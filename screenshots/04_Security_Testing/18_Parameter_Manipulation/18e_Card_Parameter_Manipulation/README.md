# Card Parameter Manipulation

## Objective

Evaluate the application's input validation, server-side request processing, authentication handling, business logic enforcement, and error handling by manipulating payment card creation parameters before submission. The assessment focused on identifying improper input validation, authentication binding weaknesses, business logic flaws, information disclosure, and server-side validation of user-controlled input.

---

## Burp Suite Tools Used

- **Proxy** – Captured payment card creation requests.
- **Target Site Map** – Identified the card creation endpoint.
- **Repeater** – Modified request parameters and replayed requests to evaluate server-side validation.

---

## Target Endpoint

```http
POST /api/Cards/
```

---

# Testing Methodology

## 1. Authentication Parameter Manipulation

Card creation requests were tested by manipulating authentication credentials between two authenticated users.

Accounts used:

- **test@gmail.com**
- **example@gmail.com**

The following combinations were tested:

- Authorization Bearer (example@gmail.com) + Cookie (test@gmail.com)
- Authorization Bearer (test@gmail.com) + Cookie (example@gmail.com)

### Observation

Both requests successfully returned:

```http
HTTP/1.1 201 Created
```

The created payment card was always associated with the user represented by the **Authorization Bearer token**, regardless of the authentication cookie supplied.

This indicates that the endpoint relies on the Authorization header to determine the authenticated user during card creation.

---

## 2. Card Number Validation

The `cardNum` parameter was tested using multiple input variations.

Test cases included:

- Valid 16-digit card number
- Less than 16 digits
- More than 16 digits
- Alphabetic characters
- Alphanumeric values
- SQL Injection payload
- XSS payload
- Parameter removed

### Observation

- Valid 16-digit card numbers were accepted successfully.

```http
HTTP/1.1 201 Created
```

- Card numbers shorter or longer than 16 digits returned:

```http
HTTP/1.1 400 Bad Request
```

- Alphabetic or alphanumeric values generated:

```http
HTTP/1.1 500 Internal Server Error
```

- SQL injection payloads generated:

```http
HTTP/1.1 500 Internal Server Error
```

- XSS payloads generated:

```http
HTTP/1.1 500 Internal Server Error
```

- Removing the `cardNum` parameter entirely still allowed card creation:

```http
HTTP/1.1 201 Created
```

The created card contained:

```json
"cardNum": null
```

These observations indicate inconsistent validation of mandatory fields.

---

## 3. Expiry Month Validation

The `expMonth` parameter was modified using different values.

Test cases included:

- Valid values (1–12)
- Empty value
- Zero
- Values greater than 12
- Negative values
- SQL Injection payload
- XSS payload
- Parameter removed

### Observation

Valid expiry months were accepted successfully.

```http
HTTP/1.1 201 Created
```

Invalid values including empty input, zero, values greater than 12, negative numbers, SQL injection, and XSS payloads returned:

```http
HTTP/1.1 400 Bad Request
```

Removing the parameter entirely still allowed successful card creation.

```http
HTTP/1.1 201 Created
```

The created card contained:

```json
"expMonth": null
```

This indicates insufficient validation of mandatory fields.

---

## 4. Expiry Year Validation

The `expYear` parameter was modified using multiple values.

Test cases included:

- Valid values (2080–2099)
- Values below 2080
- Values above 2099
- Negative values
- Empty value
- Alphabetic characters
- SQL Injection payload
- XSS payload
- Parameter removed

### Observation

Valid expiry years were accepted successfully.

```http
HTTP/1.1 201 Created
```

Invalid values generated:

```http
HTTP/1.1 400 Bad Request
```

Removing the parameter entirely still allowed successful card creation.

```http
HTTP/1.1 201 Created
```

The created card contained:

```json
"expYear": null
```

This indicates inconsistent validation of mandatory parameters.

---

## 5. Full Name Validation

The `fullName` parameter was modified using multiple input values.

Test cases included:

- Empty value
- Numeric values
- Long alphanumeric strings
- SQL Injection payload
- XSS payload
- Parameter removed

### Observation

The application accepted:

- Empty values
- Numeric values
- Long alphanumeric strings
- SQL Injection payloads
- XSS payloads
- Missing parameter

using:

```http
HTTP/1.1 201 Created
```

When the parameter was removed completely, the created card contained:

```json
"fullName": null
```

No evidence of SQL injection or JavaScript execution was observed during testing.

---

## 6. Duplicate Card Creation

The same payment card details were submitted repeatedly.

### Observation

Each request returned:

```http
HTTP/1.1 201 Created
```

The application successfully created duplicate payment cards without enforcing uniqueness validation.

This indicates insufficient business logic validation.

---

## 7. Error Handling Assessment

Malformed JSON requests were intentionally submitted.

### Observation

The application responded with:

```http
HTTP/1.1 500 Internal Server Error
```

The response exposed detailed internal exception information including:

- JavaScript SyntaxError
- Internal application paths
- Express middleware stack trace
- JSON parser implementation details

This behavior indicates improper error handling and unnecessary disclosure of implementation details.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| Authentication binding | ✅ Authorization header determines user identity |
| Valid card number | ✅ HTTP 201 Created |
| Invalid card number length | ✅ HTTP 400 Bad Request |
| Alphabetic / Alphanumeric card number | ⚠️ HTTP 500 Internal Server Error |
| Missing card number | ⚠️ HTTP 201 Created (`cardNum = null`) |
| Valid expiry month | ✅ HTTP 201 Created |
| Invalid expiry month | ✅ HTTP 400 Bad Request |
| Missing expiry month | ⚠️ HTTP 201 Created (`expMonth = null`) |
| Valid expiry year | ✅ HTTP 201 Created |
| Invalid expiry year | ✅ HTTP 400 Bad Request |
| Missing expiry year | ⚠️ HTTP 201 Created (`expYear = null`) |
| Full name validation | ⚠️ Empty, numeric, SQLi and XSS payloads accepted |
| Missing full name | ⚠️ HTTP 201 Created (`fullName = null`) |
| Duplicate card creation | ⚠️ Multiple identical cards created |
| Malformed JSON request | ⚠️ HTTP 500 Internal Server Error |

---

# Security Observations

## Authentication Binding

Testing demonstrated that payment card ownership is determined by the **Authorization Bearer token** rather than the authentication cookie.

Changing the cookie token alone did not affect which user received the newly created payment card.

---

## Server-Side Input Validation

Validation was inconsistent across multiple parameters.

Mandatory fields such as **cardNum**, **expMonth**, **expYear**, and **fullName** could be omitted while the application still created payment cards containing null values.

Additionally, certain malformed inputs generated unexpected Internal Server Errors instead of proper client validation responses.

---

## Business Logic Validation

Several business rule weaknesses were identified.

Examples include:

- Missing card number accepted.
- Missing expiry month accepted.
- Missing expiry year accepted.
- Missing full name accepted.
- Duplicate payment cards accepted.
- Empty or numeric cardholder names accepted.

These observations indicate insufficient enforcement of expected payment card validation rules.

---

## Improper Error Handling

Malformed JSON requests and several invalid card number inputs generated:

```http
HTTP/1.1 500 Internal Server Error
```

Responses exposed parser exceptions, application stack traces, and implementation details that should not be disclosed in production environments.

---

## SQL Injection Handling

SQL injection payloads submitted within payment card parameters did not result in successful SQL query manipulation.

Card number payloads generated server errors, while SQL injection payloads submitted in the cardholder name field were accepted and stored as ordinary input.

No evidence of database compromise was observed.

---

## Cross-Site Scripting Handling

XSS payloads submitted within the card number parameter generated validation errors, while XSS payloads supplied in the cardholder name field were accepted and stored as ordinary text.

No client-side JavaScript execution was observed during testing.

---

# Evidence Captured

- Authentication parameter manipulation
- Authorization header identity verification
- Card number validation
- Missing card number validation bypass
- Expiry month validation
- Missing expiry month validation bypass
- Expiry year validation
- Missing expiry year validation bypass
- Full name validation
- Duplicate payment card creation
- Malformed JSON error handling

---

# Conclusion

Card parameter manipulation testing was performed by modifying authentication credentials and multiple user-controlled parameters including **cardNum**, **expMonth**, **expYear**, and **fullName**.

The assessment identified inconsistencies in server-side validation, business rule enforcement, and error handling. Several mandatory parameters could be omitted while payment cards were still created with null values. Duplicate payment cards were accepted without uniqueness validation, and malformed requests generated internal server errors exposing implementation details.

Authentication testing demonstrated that payment card ownership is determined solely by the **Authorization Bearer token**, regardless of the authentication cookie supplied.

Overall, the assessment demonstrated manual parameter manipulation techniques using Burp Suite Repeater to evaluate authentication handling, server-side validation, business logic enforcement, input processing, and error handling within the payment card creation functionality.

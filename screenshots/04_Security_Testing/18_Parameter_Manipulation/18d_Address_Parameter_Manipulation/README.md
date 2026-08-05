# Address Parameter Manipulation

## Objective

Evaluate the application's input validation, server-side request processing, authentication handling, and business logic enforcement by manipulating address creation parameters before submission. The assessment focused on identifying improper input validation, authentication binding weaknesses, business logic flaws, error handling, information disclosure, and server-side validation of user-controlled input.

---

## Burp Suite Tools Used

- **Proxy** – Captured address creation requests.
- **Target Site Map** – Identified the address creation endpoint.
- **Repeater** – Modified request parameters and replayed requests to evaluate server-side validation.

---

## Target Endpoint

```http
POST /api/Addresss/
```

---

# Testing Methodology

## 1. Authentication Parameter Manipulation

Address creation requests were tested by manipulating authentication credentials between two authenticated users.

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

However, the created address was always associated with the user represented by the **Authorization Bearer token**, regardless of the authentication cookie.

This indicates that the endpoint primarily relies on the Authorization header to determine the authenticated user during address creation.

---

## 2. Mobile Number Validation

The `mobileNum` parameter was tested using multiple input variations.

Test cases included:

- Valid 10-digit number
- 11-digit number
- 7 to 10 digits
- 1 to 6 digits
- Alphabetic characters
- Alphanumeric values
- SQL Injection payload
- XSS payload
- Empty value
- Parameter removed

### Observation

- Valid mobile numbers were accepted successfully.

```http
HTTP/1.1 201 Created
```

- 11-digit values returned:

```http
HTTP/1.1 400 Bad Request
```

- Values containing alphabets generated:

```http
HTTP/1.1 500 Internal Server Error
```

- SQL injection payloads were rejected with:

```http
HTTP/1.1 400 Bad Request
```

- XSS payloads resulted in:

```http
HTTP/1.1 500 Internal Server Error
```

- Empty mobile number generated:

```http
HTTP/1.1 500 Internal Server Error
```

- Removing the parameter entirely still allowed address creation:

```http
HTTP/1.1 201 Created
```

These observations indicate inconsistent validation of mandatory fields.

---

## 3. Full Name Validation

The `fullName` parameter was modified using multiple input values.

Test cases included:

- Long character strings
- Numeric values
- Alphanumeric values
- Empty value
- Parameter removed
- SQL Injection payload
- XSS payload

### Observation

The application accepted:

- Long strings
- Numeric values
- Alphanumeric values
- Empty values
- Missing parameter

using:

```http
HTTP/1.1 201 Created
```

SQL injection and XSS payloads were rejected with:

```http
HTTP/1.1 400 Bad Request
```

No evidence of successful SQL injection or script execution was observed.

---

## 4. ZIP Code Validation

The `zipCode` parameter was modified using different values.

Test cases included:

- 1–8 digits
- Alphabetic values
- Empty value
- Parameter removed
- Negative numbers
- More than 9 digits

### Observation

The application accepted:

- 1–8 character values
- Alphabetic values
- Negative ZIP codes

using:

```http
HTTP/1.1 201 Created
```

More than nine digits resulted in:

```http
HTTP/1.1 400 Bad Request
```

Removing the parameter still allowed address creation:

```http
HTTP/1.1 201 Created
```

These observations indicate insufficient business validation for ZIP code values.

---

## 5. State, City and Street Address Validation

The following parameters were tested:

- state
- city
- streetAddress

Test cases included:

- SQL Injection payloads
- XSS payloads
- Empty values
- Missing values

### Observation

- SQL injection and XSS payloads submitted within **state**, **city**, and **streetAddress** were accepted successfully.

```http
HTTP/1.1 201 Created
```

The payloads were stored as ordinary text and no SQL query manipulation or JavaScript execution was observed.

- Empty **streetAddress** returned:

```http
HTTP/1.1 400 Bad Request
```

- Empty **state** and **city** were accepted successfully.

```http
HTTP/1.1 201 Created
```

---

## 6. Stored XSS Input Validation

The following payload was submitted:

```html
<script>alert(1)</script>
```

### Observation

The application responded with:

```http
HTTP/1.1 201 Created
```

The payload was stored within the submitted address field without modification.

During testing, no client-side JavaScript execution was observed.

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
- Parser implementation details

This behavior indicates improper error handling and unnecessary disclosure of implementation details.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| Authentication binding | ✅ Authorization header determines user identity |
| Valid mobile number | ✅ HTTP 201 Created |
| Invalid mobile number | ⚠️ HTTP 400 / HTTP 500 |
| Missing mobile number | ⚠️ HTTP 201 Created |
| Full name validation | ✅ Accepted multiple input formats |
| SQL Injection (Full Name) | ✅ HTTP 400 |
| XSS (Full Name) | ✅ HTTP 400 |
| ZIP code validation | ⚠️ Negative ZIP code accepted |
| Missing ZIP code | ⚠️ HTTP 201 Created |
| Street Address empty | ✅ HTTP 400 |
| State / City empty | ⚠️ HTTP 201 Created |
| SQL Injection (Address fields) | ✅ Stored as ordinary text |
| XSS (Address fields) | ✅ Stored as ordinary text |
| Stored XSS payload | ✅ Stored, execution not observed |
| Malformed JSON request | ⚠️ HTTP 500 Internal Server Error |

---

# Security Observations

## Authentication Binding

Testing demonstrated that address ownership is determined by the **Authorization Bearer token** rather than the authentication cookie.

Changing the cookie token alone did not affect which user received the newly created address.

---

## Server-Side Input Validation

Validation was inconsistent across multiple parameters.

While some invalid inputs correctly generated client errors (**HTTP 400**), other malformed or missing values resulted in successful address creation or unexpected internal server errors.

---

## Business Logic Validation

Several business rule weaknesses were observed.

Examples include:

- Missing mobile number accepted.
- Missing ZIP code accepted.
- Empty state and city accepted.
- Negative ZIP code accepted.
- Numeric-only names accepted.

These observations indicate insufficient validation of expected address data.

---

## Improper Error Handling

Malformed JSON and certain invalid parameter values generated:

```http
HTTP/1.1 500 Internal Server Error
```

Responses exposed internal parser exceptions, application stack traces, and implementation details that should not be disclosed in production environments.

---

## SQL Injection Handling

SQL injection payloads submitted within address fields were processed as ordinary input.

No SQL query manipulation or database compromise was observed during testing.

---

## Cross-Site Scripting Handling

JavaScript payloads submitted within address fields were accepted and stored as user input.

During testing, no client-side JavaScript execution was observed.

---

# Evidence Captured

- Authentication parameter manipulation
- Authorization header identity verification
- Mobile number validation
- Stored XSS input acceptance
- Malformed JSON error handling
- Negative ZIP code acceptance

---

# Conclusion

Address parameter manipulation testing was performed by modifying authentication credentials and multiple user-controlled parameters including **fullName**, **mobileNum**, **zipCode**, **streetAddress**, **city**, and **state**.

The assessment identified inconsistencies in server-side validation, business rule enforcement, and error handling. Several invalid or missing parameters were accepted successfully, while malformed requests generated internal server errors exposing implementation details.

Authentication testing demonstrated that address ownership is determined by the **Authorization Bearer token**, regardless of the authentication cookie supplied with the request.

Overall, the assessment demonstrated manual parameter manipulation techniques using Burp Suite Repeater to evaluate authentication handling, server-side validation, business logic enforcement, input processing, and error handling within the address creation functionality.

# Feedback Parameter Manipulation

## Objective

Evaluate the application's input validation and server-side handling of feedback submission parameters by manipulating request values before submission. The assessment focused on identifying improper input validation, business logic weaknesses, error handling, information disclosure, and server-side validation of user-supplied parameters.

---

## Burp Suite Tools Used

- **Proxy** – Captured feedback submission requests.
- **Target Site Map** – Identified the feedback submission endpoint.
- **Repeater** – Modified request parameters and replayed requests to evaluate server-side validation.

---

## Target Endpoint

```http
POST /api/Feedbacks/
```

---

# Testing Methodology

## 1. UserId Parameter Manipulation

The `UserId` parameter was modified using different valid and invalid values.

Test cases included:

- Valid UserId
- Invalid UserId
- Negative UserId
- UserId removed

### Observation

A valid UserId resulted in:

```http
HTTP/1.1 201 Created
```

Invalid UserId values produced:

```http
HTTP/1.1 500 Internal Server Error
```

The response exposed an internal SQLite database error:

```text
SQLITE_CONSTRAINT: FOREIGN KEY constraint failed
```

When the `UserId` parameter was completely removed, the application responded with:

```http
HTTP/1.1 201 Created
```

and created the feedback entry with:

```json
"UserId": null
```

This indicates insufficient validation of mandatory parameters.

---

## 2. CAPTCHA Validation

The `captcha` parameter was modified with incorrect values.

### Observation

The application responded with:

```http
HTTP/1.1 401 Unauthorized
```

Response message:

```text
Wrong answer to CAPTCHA. Please try again.
```

The application correctly validated CAPTCHA values before processing feedback submissions.

---

## 3. Rating Parameter Manipulation

The `rating` parameter was modified using different numeric and non-numeric values.

Test cases included:

```text
-999
3532
abc
rating removed
```

### Observation

Arbitrary numeric values were accepted successfully.

```http
HTTP/1.1 201 Created
```

When the rating was supplied as text:

```text
abc
```

or completely removed, the application responded with:

```http
HTTP/1.1 500 Internal Server Error
```

indicating insufficient server-side validation for mandatory numeric fields.

---

## 4. SQL Injection Input Validation

SQL injection payloads were submitted within the feedback parameters.

Payload tested:

```text
'--
```

### Observation

The application responded with:

```http
HTTP/1.1 201 Created
```

The payload was stored as ordinary text and no SQL query manipulation or database error was observed.

Successful SQL injection was not identified during testing.

---

## 5. Cross-Site Scripting (XSS) Input Validation

The following payload was submitted:

```html
<img src=x onerror=alert(1)>
```

### Observation

The application responded with:

```http
HTTP/1.1 201 Created
```

The returned response contained an empty comment value, indicating that the malicious HTML payload was filtered before storage.

No JavaScript execution was observed during testing.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| Valid UserId | ✅ HTTP 201 Created |
| Invalid UserId | ⚠️ HTTP 500 Internal Server Error |
| Missing UserId | ⚠️ HTTP 201 Created (`UserId = null`) |
| Invalid CAPTCHA | ✅ HTTP 401 Unauthorized |
| Invalid Rating Value | ⚠️ HTTP 201 Created |
| Invalid Rating Datatype | ⚠️ HTTP 500 Internal Server Error |
| Missing Rating | ⚠️ HTTP 500 Internal Server Error |
| SQL Injection Payload | ✅ Stored as plain text |
| XSS Payload | ✅ Payload filtered, execution not observed |

---

# Security Observations

## Server-Side Input Validation

The application correctly validated CAPTCHA values before processing requests.

However, mandatory fields such as `UserId` and `rating` were not consistently validated, resulting in multiple server-side exceptions and improper request handling.

---

## Improper Error Handling

Invalid `UserId` values and malformed `rating` parameters generated:

```http
HTTP/1.1 500 Internal Server Error
```

The responses exposed internal SQLite database errors and server-side exception information instead of returning appropriate client validation errors.

---

## Business Logic Validation

The application accepted arbitrary numeric values for the `rating` parameter without enforcing expected limits.

Additionally, removing the `UserId` parameter resulted in feedback creation with a `null` user reference.

These observations indicate insufficient business rule validation.

---

## SQL Injection Handling

SQL injection payloads were processed as ordinary user input and no evidence of SQL query manipulation or database compromise was observed.

---

## Cross-Site Scripting Handling

The submitted XSS payload was filtered before storage and no client-side script execution was observed during testing.

This behavior indicates that the tested payload was mitigated by the application.

---

# Evidence Captured

- UserId parameter manipulation
- Invalid UserId database constraint error
- Missing UserId behaviour
- CAPTCHA validation
- Rating parameter manipulation
- SQL injection input validation
- XSS payload filtering

---

# Conclusion

Feedback parameter manipulation testing was performed by modifying user-controlled request parameters including `UserId`, `captcha`, `rating`, and `comment`.

The assessment identified weaknesses in server-side input validation and business logic enforcement. Invalid `UserId` values and malformed `rating` parameters resulted in internal server errors, while removing the `UserId` parameter allowed feedback creation with a null user reference.

The application correctly enforced CAPTCHA validation and successfully handled the tested SQL injection and XSS payloads without evidence of successful exploitation.

Overall, the assessment demonstrated manual parameter manipulation techniques using Burp Suite Repeater to evaluate server-side validation, error handling, business logic enforcement, and input processing within the feedback submission functionality.

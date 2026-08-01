
# Search Parameter Manipulation

## Objective

Evaluate the application's input validation and server-side handling of search parameters by manipulating the search query used to retrieve products. The assessment focused on identifying improper input validation, error handling, information disclosure, and the application's response to malformed or unexpected user input.

---

## Burp Suite Tools Used

- **Proxy** – Captured search requests.
- **Target Site Map** – Identified the product search endpoint.
- **Repeater** – Modified search parameters and replayed requests to evaluate server-side input validation.

---

## Target Endpoint

```http
GET /rest/products/search?q=
```

---

# Testing Methodology

## 1. Empty Search Parameter

The search parameter was submitted without any value.

Modified request:

```http
GET /rest/products/search?q=
```

### Observation

The application responded with:

```http
HTTP/1.1 200 OK
```

and returned the complete list of available products.

---

## 2. SQL Injection Testing

Multiple SQL injection payloads were submitted through the search parameter.

Payloads tested included:

```text
'
'--
```

### Observation

Payload:

```text
'
```

Result:

```http
HTTP/1.1 200 OK
```

Returned an empty result set.

---

Payload:

```text
'--
```

Result:

```http
HTTP/1.1 500 Internal Server Error
```

The response disclosed internal SQLite error information including:

- SQLITE_ERROR
- SQL query executed by the application
- Database error message
- Internal stack trace

Example:

```text
SQLITE_ERROR: incomplete input

SELECT * FROM Products
WHERE ((name LIKE '%'--%' OR description LIKE '%'--%')
AND deletedAt IS NULL)
```

This behavior exposed internal database implementation details through verbose error messages.

---

## 3. Cross-Site Scripting (XSS) Input Validation

The following payload was submitted:

```html
<script>alert(1)</script>
```

### Observation

The application responded with:

```http
HTTP/1.1 200 OK
```

No JavaScript execution occurred and no products were returned.

The supplied payload was not reflected or executed within the application.

---

## 4. URL Encoded Input

The following URL-encoded value was tested:

```text
%61%70%70%6c%65
```

which represents:

```text
apple
```

### Observation

The application successfully decoded the input and returned the expected search results.

Response:

```http
HTTP/1.1 200 OK
```

---

## 5. Wildcard Character Testing

Multiple wildcard and special characters were tested, including:

```text
%
#
+
_
!
@
$
&
*
(
)
-
```

### Observation

Different wildcard characters produced different search behaviours.

Examples observed:

- `%` returned all products.
- `#` returned all products.
- `+` returned all products.
- `_` returned all products.
- Some characters returned partial results.
- Other characters returned an empty result set.

The application continued processing all requests without server-side failures.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| Empty search parameter | ✅ HTTP 200 OK |
| SQL Injection (`'`) | ✅ Empty result |
| SQL Injection (`'--`) | ⚠️ HTTP 500 Internal Server Error |
| XSS payload | ✅ Payload not executed |
| URL encoded search | ✅ Successfully decoded |
| Wildcard character testing | ✅ Various search behaviours observed |

---

# Security Observations

## Input Validation

The application handled empty input, long input, URL-encoded input, and XSS payloads without crashing or executing client-side code.

---

## Verbose Error Disclosure

The payload:

```text
'--
```

generated an internal database error.

The application exposed:

- SQLite database type
- SQL query
- Error message
- Stack trace

Although successful SQL injection was not observed, exposing internal database errors provides attackers with useful reconnaissance information regarding the application's backend implementation.

---

## Search Behaviour

Different wildcard characters produced varying search results, including returning all available products for specific characters.

The application remained stable throughout testing and consistently returned HTTP responses without unexpected crashes, except during the malformed SQL injection payload.

---

# Evidence Captured

- Empty search parameter behaviour
- SQL injection error disclosure
- XSS payload validation
- URL encoded search behaviour
- Wildcard character search behaviour

---

# Conclusion

Search parameter manipulation testing was performed by modifying the search query parameter with malformed, encoded, and potentially malicious input values.

The application demonstrated stable handling of most input variations including XSS payloads, URL-encoded values, and wildcard characters.

Testing identified that the malformed SQL injection payload (`'--`) triggered an internal SQLite error and exposed verbose database error information, including the executed SQL query and stack trace. While no SQL injection vulnerability was confirmed, the exposed error details represent an information disclosure issue that could assist attackers during reconnaissance.

Overall, the assessment demonstrated manual input validation testing techniques using Burp Suite Repeater to evaluate search functionality and server-side parameter handling.

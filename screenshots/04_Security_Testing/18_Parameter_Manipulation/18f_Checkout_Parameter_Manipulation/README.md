# Basket Parameter Manipulation

## Objective

Evaluate the application's server-side validation and handling of basket-related parameters by manipulating the checkout request. The assessment focused on identifying improper input validation, parameter validation weaknesses, error handling, and the application's response to malformed or unexpected parameter values during the checkout process.

---

## Burp Suite Tools Used

- **Proxy** – Captured basket checkout requests.
- **Target Site Map** – Identified the checkout endpoint.
- **Repeater** – Modified basket-related parameters and replayed requests to evaluate server-side validation.

---

## Target Endpoint

```http
POST /rest/basket/{basketId}/checkout
```

---

# Testing Methodology

## 1. Authentication Header Validation

The checkout request was tested by modifying the authentication credentials.

The following scenarios were tested:

- Modified Authorization Bearer token
- Modified authentication Cookie
- Authorization header replaced
- Cookie replaced

### Observation

The application continued processing requests only when valid authentication credentials were supplied.

Requests made using valid authentication sessions returned:

```http
HTTP/1.1 200 OK
```

When the authenticated basket had already been checked out, the application generated a new order containing no products because the basket was empty.

---

## 2. Checkout Parameter Validation

The following request body parameters were manually manipulated:

- `paymentId`
- `addressId`
- `deliveryMethodId`
- `couponData`

Each parameter was tested individually using:

- Empty values
- Null values
- Removal of the parameter
- Negative numbers
- Alphabetic characters
- Alphanumeric values
- Special characters
- SQL Injection payloads
- Cross-Site Scripting (XSS) payloads

### Observation

For all tested parameter variations, the application consistently returned:

```http
HTTP/1.1 200 OK
```

Observed behaviour:

- If the basket contained products, checkout completed successfully.
- If the basket had already been checked out, a new empty order was created.
- None of the manipulated parameter values prevented the checkout operation.

No client-side script execution or SQL injection behavior was observed through these parameters.

---

## 3. Empty Request Body

The entire JSON request body was removed before replaying the request.

Modified request:

```json
{}
```

### Observation

The application successfully processed the checkout request.

Response:

```http
HTTP/1.1 200 OK
```

When products were present in the basket, checkout completed successfully using the existing basket information.

When the basket was empty, an empty order was generated.

This behaviour indicates that the application primarily relies on the basket state rather than the supplied request body parameters during checkout.

---

## 4. Malformed JSON Testing

The JSON request body was intentionally modified to contain invalid JSON syntax.

Example:

```json
{
  "paymentId":"7",
  "addressId":"7",
```

### Observation

The application returned:

```http
HTTP/1.1 500 Internal Server Error
```

The response disclosed internal implementation details including:

- JSON parsing error
- Server-side stack trace
- Internal file paths
- Framework error information

Example:

```text
Unexpected token '{'
... is not valid JSON
```

This behavior resulted in verbose server-side error disclosure.

---

## 5. Invalid Basket Identifier

The basket identifier contained within the request URL was replaced with a non-existent basket identifier.

Modified request:

```http
POST /rest/basket/10/checkout
```

### Observation

The application returned:

```http
HTTP/1.1 500 Internal Server Error
```

The response disclosed:

```text
Basket with id=10 does not exist.
```

along with an internal stack trace.

The server correctly rejected the invalid basket identifier; however, verbose error information was exposed within the response.

---

# Summary of Tests Performed

| Test | Result |
|------|--------|
| Authentication header manipulation | ✅ Valid sessions processed successfully |
| paymentId manipulation | ✅ HTTP 200 OK |
| addressId manipulation | ✅ HTTP 200 OK |
| deliveryMethodId manipulation | ✅ HTTP 200 OK |
| couponData manipulation | ✅ HTTP 200 OK |
| Empty request body | ✅ HTTP 200 OK |
| Malformed JSON | ⚠️ HTTP 500 Internal Server Error |
| Invalid basket identifier | ⚠️ HTTP 500 Internal Server Error |

---

# Security Observations

## Parameter Validation

Testing demonstrated that the checkout process continued successfully despite extensive manipulation of request body parameters including empty values, null values, removed parameters, SQL injection payloads, XSS payloads, numeric manipulation, and special characters.

The application primarily relied on the authenticated basket state when processing checkout requests.

---

## Verbose Error Disclosure

Submitting malformed JSON generated an internal server error.

The response exposed:

- JSON parser error
- Internal stack trace
- Framework implementation details
- Server file paths

Such verbose error messages provide useful reconnaissance information regarding the application's backend implementation.

---

## Invalid Resource Handling

Replacing the basket identifier with a non-existent value resulted in:

```http
HTTP/1.1 500 Internal Server Error
```

The response disclosed internal error information, including the message indicating that the basket did not exist.

Although the application correctly rejected the invalid basket identifier, returning an internal server error with detailed exception information may expose unnecessary implementation details.

---

# Evidence Captured

- Authentication header validation
- Checkout parameter manipulation
- Empty request body behaviour
- Malformed JSON error disclosure
- Invalid basket identifier handling

---

# Conclusion

Basket parameter manipulation testing was performed by modifying authentication credentials, checkout parameters, request body content, and basket identifiers to evaluate server-side validation during the checkout process.

The application consistently processed checkout requests despite extensive manipulation of request body parameters, indicating that the supplied checkout parameters had limited influence over the checkout operation and that processing primarily depended on the authenticated basket state.

Testing also identified two error-handling issues. Malformed JSON input generated verbose server-side error messages exposing internal implementation details, while invalid basket identifiers resulted in detailed exception messages and stack traces. Although no direct parameter manipulation vulnerability was confirmed through the tested request body parameters, the observed information disclosure may assist attackers during reconnaissance.

Overall, the assessment demonstrated manual parameter manipulation techniques using Burp Suite Repeater to evaluate server-side validation, request processing, and error handling within the basket checkout functionality.

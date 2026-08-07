# Checkout Parameter Manipulation

## Objective

Evaluate the application's input validation and server-side handling of checkout parameters by manipulating request parameters used during the order checkout process. The assessment focused on identifying improper input validation, error handling, parameter validation, information disclosure, and the application's response to malformed or unexpected user input.

---

## Burp Suite Tools Used

- **Proxy** – Captured checkout requests.
- **Target Site Map** – Identified the checkout endpoint.
- **Repeater** – Modified checkout parameters and replayed requests to evaluate server-side input validation.

---

## Target Endpoint

```http
POST /rest/basket/{basketId}/checkout
```

---

# Testing Methodology

## 1. Authentication Verification

The checkout request was tested by modifying authentication credentials.

The following scenarios were tested:

- Authorization Bearer token modified
- Authentication Cookie modified
- Authorization Bearer token removed
- Authentication Cookie removed

### Observation

The application continued processing the checkout request using the authenticated session.

Observed responses included:

```http
HTTP/1.1 200 OK
```

When the associated basket contained products, checkout completed successfully.

When the basket had already been checked out, the application still returned **HTTP 200 OK**, but generated an empty order because no products remained in the basket.

---

## 2. Checkout Parameter Manipulation

The following request parameters were manually modified during testing:

- paymentId
- addressId
- deliveryMethodId
- couponData

The following input variations were tested:

- Empty values
- Null values
- Parameter removal
- Negative values
- Alphabetic input
- Alphanumeric input
- Special characters
- SQL Injection payloads
- Cross-Site Scripting (XSS) payloads

### Observation

For all tested input variations, the application responded with:

```http
HTTP/1.1 200 OK
```

No unexpected server-side validation failures were observed.

If the basket contained products, checkout completed successfully.

If the basket was empty, the application completed checkout and generated an empty order.

---

## 3. Invalid Basket Identifier

The basket identifier contained within the request URL was modified to a non-existent basket identifier.

Modified request example:

```http
POST /rest/basket/10/checkout
```

### Observation

The application responded with:

```http
HTTP/1.1 500 Internal Server Error
```

The response disclosed an application error indicating:

```text
Basket with id=10 does not exist.
```

The response also included an internal application stack trace.

This behavior exposed internal implementation details instead of returning a generic client error.

---

## 4. Malformed JSON Testing

The JSON request body was intentionally modified to contain invalid JSON syntax.

Example modifications included:

- Missing quotation marks
- Missing commas
- Incorrect braces
- Invalid JSON structure

### Observation

The application responded with:

```http
HTTP/1.1 500 Internal Server Error
```

The response exposed detailed parser error information including:

- Unexpected token
- JSON parsing exception
- Internal stack trace

The application returned verbose internal error messages rather than a generic validation error.

---

## 5. Cross-Site Scripting (XSS) Input Validation

Cross-Site Scripting payloads were submitted through multiple checkout parameters.

Example payload:

```html
<script>alert(1)</script>
```

### Observation

The application responded with:

```http
HTTP/1.1 200 OK
```

The payload was not reflected within the application and no JavaScript execution occurred.

Checkout behavior remained unchanged.

---

# Summary of Tests Performed

| Test | Result |
|------|--------|
| Authentication manipulation | ✅ HTTP 200 OK |
| paymentId manipulation | ✅ HTTP 200 OK |
| addressId manipulation | ✅ HTTP 200 OK |
| deliveryMethodId manipulation | ✅ HTTP 200 OK |
| couponData manipulation | ✅ HTTP 200 OK |
| Invalid basket identifier | ⚠️ HTTP 500 Internal Server Error |
| Malformed JSON | ⚠️ HTTP 500 Internal Server Error |
| XSS payload | ✅ Payload not executed |

---

# Security Observations

## Input Validation

The application accepted multiple modified parameter values including empty, null, negative, alphanumeric, special character, SQL injection, and XSS payloads without interrupting the checkout workflow.

No client-side script execution was observed during XSS testing.

---

## Verbose Error Disclosure

Submitting malformed JSON generated an internal server error.

The application exposed detailed JSON parsing exceptions together with internal stack trace information.

Similarly, supplying a non-existent basket identifier generated an internal server error revealing application-specific error messages and implementation details.

Although these behaviors did not result in unauthorized access during this assessment, verbose error messages provide useful reconnaissance information that could assist an attacker in understanding backend application logic.

---

## Checkout Behaviour

The checkout endpoint consistently returned successful responses when processing valid authenticated requests, even when modified parameter values were supplied.

Checkout behavior primarily depended on whether products existed within the associated basket.

When products were present, an order was successfully created.

When the basket was empty, checkout still completed successfully and generated an empty order.

---

# Evidence Captured

- Authentication manipulation behaviour
- Checkout parameter manipulation
- Invalid basket identifier handling
- Malformed JSON error disclosure
- XSS payload validation

---

# Conclusion

Checkout parameter manipulation testing was performed by modifying authentication values, checkout parameters, request structure, and malformed input values submitted to the checkout endpoint.

The application demonstrated stable handling of most modified parameter values including empty, null, negative, alphanumeric, SQL injection, and XSS payloads.

Testing identified that malformed JSON requests and invalid basket identifiers triggered **HTTP 500 Internal Server Error** responses exposing verbose application error messages and internal stack traces. While these issues did not directly result in unauthorized access during parameter validation testing, the disclosed information represents an information disclosure weakness that could assist attackers during reconnaissance.

Overall, the assessment demonstrated manual input validation testing techniques using Burp Suite Repeater to evaluate checkout functionality and server-side parameter handling.

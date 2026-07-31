# Basket Parameter Manipulation

## Objective

Evaluate the application's server-side validation and business logic by manually manipulating request parameters associated with shopping basket operations. The assessment focused on verifying basket ownership validation, handling of missing parameters, and input validation for quantity values.

---

## Burp Suite Tools Used

- **Proxy** – Captured basket-related HTTP requests.
- **Target Site Map** – Identified basket management endpoints.
- **Repeater** – Modified request parameters and replayed requests to evaluate server-side validation.

---

## Target Endpoint

```http
POST /api/BasketItems/
```

---

# Testing Methodology

## 1. Basket Ownership Validation

The captured basket request was manually modified by replacing authentication credentials and basket identifiers belonging to different user accounts.

The following parameters were manipulated:

- Authorization Bearer Token
- Authentication Cookie
- BasketId

### Observation

When authentication credentials and the supplied `BasketId` belonged to different user accounts, the application rejected the request with:

```http
HTTP/1.1 401 Unauthorized
```

Response:

```json
{
  "error": "Invalid BasketId"
}
```

This behavior indicates that the application validates basket ownership before allowing modifications to shopping cart contents.

---

## 2. BasketId Removal Behaviour

The `BasketId` parameter was completely removed from the request body before replaying the request.

Modified request body:

```json
{
  "ProductId": 5,
  "quantity": 1
}
```

### Observation

The application responded with:

```http
HTTP/1.1 200 OK
```

However, manual verification confirmed that no product was added to the authenticated user's basket.

This indicates that although the request was accepted syntactically, the server did not perform the requested basket modification when the required `BasketId` parameter was absent.

---

## 3. Quantity Parameter Manipulation

The quantity parameter was modified to an invalid negative value.

Modified request:

```json
{
  "ProductId": 5,
  "BasketId": 6,
  "quantity": -3999999
}
```

### Observation

The server returned:

```http
HTTP/1.1 200 OK
```

The application accepted the negative quantity and updated the shopping basket accordingly.

Observed behaviour included:

- Negative item quantity displayed
- Negative basket total calculated
- Checkout functionality remained available

The application did not enforce server-side validation to prevent invalid quantity values.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| Basket ownership validation | ✅ HTTP 401 Unauthorized |
| BasketId removal | ✅ HTTP 200 OK (No basket modification observed) |
| Negative quantity manipulation | ✅ HTTP 200 OK |
| Authentication token manipulation | ✅ Tested |
| BasketId manipulation | ✅ Tested |
| Quantity manipulation | ✅ Tested |

---

# Security Observations

## Basket Ownership Validation

The application successfully enforced basket ownership by rejecting requests where the supplied `BasketId` did not correspond to the authenticated user's session.

This behaviour helps prevent unauthorized modification of another user's shopping basket.

---

## Missing Parameter Handling

Removing the `BasketId` parameter resulted in an HTTP 200 OK response; however, the basket contents remained unchanged after verification.

This indicates that the application handled the malformed request without performing the requested business operation.

---

## Business Logic Validation

The application accepted negative quantity values without validation.

Accepting invalid quantity values may lead to:

- Incorrect basket calculations
- Negative order totals
- Business logic abuse
- Financial inconsistencies

Although this behaviour exists within the OWASP Juice Shop training environment, similar behaviour in a production application could introduce significant business logic vulnerabilities.

---

# Evidence Captured

- Basket ownership validation using modified authentication credentials
- Invalid BasketId rejection
- BasketId removal behaviour
- Negative quantity parameter manipulation
- Updated basket displaying negative quantity and negative total

---

# Conclusion

Manual parameter manipulation testing was performed against the basket management endpoint by modifying authentication credentials, basket identifiers, and quantity values.

The application correctly enforced basket ownership validation by rejecting unauthorized basket modification attempts.

Requests with a missing `BasketId` parameter were accepted syntactically but did not result in any basket modification.

Business logic testing identified that negative quantity values were accepted without server-side validation, allowing invalid basket calculations and negative order totals.

Overall, the assessment demonstrated parameter tampering techniques including ownership validation, missing parameter handling, and business logic manipulation using Burp Suite Repeater.

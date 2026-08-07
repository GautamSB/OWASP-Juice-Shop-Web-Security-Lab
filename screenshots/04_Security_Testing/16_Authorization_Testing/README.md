# 16_Authorization_Testing

## Objective

Evaluate the application's authorization mechanisms by verifying authentication handling, JWT validation, user identity verification, access control for protected and administrative API endpoints, and object-level authorization. The assessment focused on understanding how the application authenticates users, validates session information, and restricts access to sensitive resources belonging to other users.

---

## Burp Suite Tools Used

- **Proxy** – Captured authentication requests and JWT tokens.
- **Target Site Map** – Identified API endpoints for authorization assessment.
- **Repeater** – Replayed and modified HTTP requests to validate authorization behavior.

---

## Target Endpoints

```http
POST /rest/user/login
GET  /rest/user/whoami
GET  /rest/admin/application-configuration
POST /rest/basket/{basketId}/checkout
```

---

# Testing Methodology

## 1. User Authentication Verification

Two standard user accounts were created and authenticated independently.

- **test@gmail.com**
- **example@gmail.com**

JWT tokens generated after successful authentication were captured using Burp Suite and reused throughout the authorization assessment.

---

## 2. JWT Identity Verification

The following endpoint was used to verify authenticated user identity.

```http
GET /rest/user/whoami
```

The following scenarios were tested:

- Valid JWT
- JWT from another authenticated user
- Authorization header replacement
- Cookie token replacement
- Authorization header removal
- Cookie token removal
- Removal of both Authorization header and Cookie token

### Observation

Testing showed that the endpoint determines the authenticated user primarily from the authentication cookie.

Observed behavior:

- Valid Cookie + Valid JWT → HTTP 200 OK with authenticated user information.
- Valid Cookie + Different JWT → Response reflected the identity associated with the cookie token.
- Authorization header removed → HTTP 200 OK with authenticated user information.
- Cookie removed (Bearer token only) → HTTP 200 OK with an empty user object (`{"user":{}}`).
- Authorization header and Cookie both removed → HTTP 200 OK with an empty user object (`{"user":{}}`).

These observations indicate that the `/rest/user/whoami` endpoint relies on the cookie-based authentication session to establish user identity, while the Authorization header alone was insufficient to authenticate the request.

---

## 3. Administrative Endpoint Discovery

Application mapping using Burp Suite Target Site Map identified the following administrative endpoint:

```http
GET /rest/admin/application-configuration
```

The endpoint was selected for manual authorization assessment using Burp Repeater.

---

## 4. Administrative Endpoint Authorization Testing

The administrative configuration endpoint was tested under multiple authentication scenarios.

Performed tests included:

- Valid user JWT
- Different user JWT
- Modified JWT
- Authorization header removed
- Cookie token removed
- Both Authorization header and Cookie token removed

### Observation

During testing, the endpoint consistently returned:

```http
HTTP/1.1 200 OK
```

and disclosed application configuration information regardless of the presence or absence of authentication credentials.

The returned response included application configuration details such as feature settings, application metadata, deployment configuration, and other environment-related information.

Based on the observed behavior, the endpoint remained publicly accessible during testing and did not require authentication to retrieve the configuration data.

---

## 5. Object-Level Authorization (IDOR) Assessment

The checkout functionality was evaluated to determine whether the application validates ownership of basket resources before processing an order.

The following endpoint was tested:

```http
POST /rest/basket/{basketId}/checkout
```

Two authenticated user accounts were used:

- **test@gmail.com**
- **example@gmail.com**

The authenticated session remained as **test@gmail.com**.

The **basketId** contained in the request URL was modified from the authenticated user's basket to the basket belonging to **example@gmail.com**.

No changes were made to:

- Authorization Bearer Token
- Authentication Cookie
- Session information

Only the basket identifier was modified.

### Observation

The modified request successfully returned:

```http
HTTP/1.1 200 OK
```

The application processed the checkout request using the modified basket identifier.

Observed behavior:

- Checkout completed successfully.
- Products belonging to **example@gmail.com** were included in the generated order.
- The newly generated order appeared in the order history of the authenticated **test@gmail.com** account.
- The basket belonging to **example@gmail.com** was emptied after checkout.

The application did not verify whether the supplied basket identifier belonged to the authenticated user before completing the checkout process.

This behavior demonstrates an **Insecure Direct Object Reference (IDOR)** resulting from insufficient server-side authorization checks.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| Login as test@gmail.com | ✅ Successful |
| Login as example@gmail.com | ✅ Successful |
| JWT identity verification | ✅ Successful |
| User JWT replacement | ✅ Tested |
| Authorization header manipulation | ✅ Tested |
| Cookie token manipulation | ✅ Tested |
| Authorization header removed | ✅ Tested |
| Cookie token removed | ✅ Tested |
| Authorization header + Cookie removed (`/rest/user/whoami`) | ✅ Returned HTTP 200 OK with empty user object |
| Authorization header + Cookie removed (`/rest/admin/application-configuration`) | ✅ Returned HTTP 200 OK with configuration data |
| Basket ID manipulation during checkout | ⚠️ HTTP 200 OK – Another user's basket successfully checked out |

---

# Security Observations

## Cookie-Based Authentication Behaviour

Testing of the `/rest/user/whoami` endpoint demonstrated that authenticated user identification relied primarily on the authentication cookie.

Removing the Authorization header did not affect user identification while a valid authentication cookie remained present.

When the cookie was removed, the endpoint continued responding with **HTTP 200 OK**, but returned an empty user object instead of authenticated user information.

This behavior indicates that the endpoint uses the cookie-based session to establish the authenticated user context.

---

## Administrative Configuration Exposure

The endpoint

```http
GET /rest/admin/application-configuration
```

returned application configuration information regardless of whether authentication credentials were supplied.

The disclosed information included application metadata, feature configuration, deployment settings, and other configuration values.

Although this behavior may be intentional within the OWASP Juice Shop training environment, exposing such configuration data in a production environment could assist attackers during reconnaissance by revealing implementation details.

---

## Insecure Direct Object Reference (IDOR)

Manual manipulation of the **basketId** within the checkout request allowed an authenticated user to successfully process another user's shopping basket.

The server accepted the modified basket identifier without verifying ownership of the referenced resource.

As a result:

- Another user's basket contents were successfully checked out.
- The generated order appeared in the authenticated user's order history.
- The original owner's basket was emptied after checkout.

These observations indicate insufficient object-level authorization and demonstrate a **Broken Access Control (IDOR)** vulnerability.

---

# Evidence Captured

- Successful login using **test@gmail.com**
- Successful login using **example@gmail.com**
- JWT identity verification using `/rest/user/whoami`
- Cookie-based authentication verification
- Administrative endpoint discovery using Burp Target Site Map
- Authorization assessment of `/rest/admin/application-configuration`
- Checkout request with modified **basketId**
- Successful checkout of another user's basket
- Order history showing unauthorized checkout results

---

# Conclusion

Authorization testing was performed using Burp Suite by manually modifying authentication headers, cookie values, JWT tokens, and user-controlled object identifiers to evaluate how the application enforced authorization.

The `/rest/user/whoami` endpoint consistently relied on the authentication cookie to establish user identity. Removing the Authorization header alone did not affect authentication, whereas removing the cookie resulted in the endpoint returning an empty user object despite responding with **HTTP 200 OK**.

The `/rest/admin/application-configuration` endpoint remained accessible under all tested authentication scenarios and consistently returned application configuration information.

Additionally, the checkout functionality was found to be vulnerable to **Insecure Direct Object Reference (IDOR)**. By modifying the `basketId` within the checkout request, it was possible to process another authenticated user's basket without changing the authenticated session. The server failed to verify ownership of the referenced basket before processing the checkout request, allowing unauthorized access to another user's resources.

Overall, the assessment demonstrated manual authorization testing techniques including JWT verification, cookie manipulation, identity validation, administrative endpoint assessment, and object-level authorization testing using Burp Suite.

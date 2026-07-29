# 16_Authorization_Testing

## Objective

Evaluate the application's authorization controls by verifying how protected resources handle authenticated and unauthenticated requests, JWT validation, user identity verification, and access to administrative endpoints. The assessment focused on identifying improper access control, authentication enforcement, and potential information disclosure.

---

## Burp Suite Tools Used

- **Proxy** – Captured authenticated requests and JWT tokens.
- **Target Site Map** – Identified endpoints for authorization assessment.
- **Repeater** – Replayed and modified requests to verify authorization behavior.

---

## Target Endpoints

```http
POST /rest/user/login
GET  /rest/user/whoami
GET  /rest/admin/application-configuration
```

---

# Testing Methodology

## 1. User Authentication Verification

Two standard user accounts were created and authenticated independently.

- **test@gmail.com**
- **example@gmail.com**

JWT tokens generated after successful authentication were captured using Burp Suite and reused during authorization testing.

---

## 2. JWT Identity Verification

The endpoint

```http
GET /rest/user/whoami
```

was used to verify the identity associated with the supplied JWT.

Testing included:

- Valid JWT
- JWT from another user
- Cookie token replacement
- Authorization header replacement

### Observation

The application identified the authenticated user based on the JWT supplied in the authentication cookie during testing. Swapping JWTs resulted in the response reflecting the identity associated with the active authentication token.

---

## 3. Administrative Endpoint Discovery

Application mapping using Burp Target Site Map identified the following endpoint:

```http
GET /rest/admin/application-configuration
```

The endpoint was selected for manual authorization testing using Burp Repeater.

---

## 4. Administrative Endpoint Authorization Testing

The administrative configuration endpoint was tested under multiple authentication scenarios.

Performed tests:

- Valid user JWT
- Different user JWT
- Modified JWT
- Authorization header removed
- Cookie token removed
- Both Authorization header and Cookie token removed

### Observation

The endpoint consistently returned:

```http
HTTP/1.1 200 OK
```

and disclosed application configuration information even when authentication tokens were removed.

The observed behavior indicates that this endpoint is publicly accessible in the tested Juice Shop instance and does not enforce authentication for the returned configuration data.

---

## 5. Authentication Enforcement Verification

The endpoint

```http
GET /rest/user/whoami
```

was tested after removing both:

- Authorization: Bearer header
- Cookie authentication token

### Observation

The request failed because authentication information was missing, demonstrating that the endpoint correctly requires an authenticated session before returning user information.

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
| Missing Authorization header | ✅ Tested |
| Missing Cookie token | ✅ Tested |
| Access to `/rest/user/whoami` without authentication | ✅ Authentication Required |
| Access to `/rest/admin/application-configuration` without authentication | ✅ Publicly Accessible (Observed) |

---

# Security Observations

### Authentication Enforcement

The `/rest/user/whoami` endpoint correctly enforced authentication by rejecting requests when authentication tokens were removed.

---

### Administrative Configuration Exposure

The endpoint

```http
GET /rest/admin/application-configuration
```

returned application configuration information regardless of the presence of authentication tokens.

The disclosed information included application configuration values, feature settings, and deployment-related metadata.

This behavior may assist attackers during reconnaissance by exposing implementation details.

---

# Evidence Captured

- Login using **test@gmail.com**
- Login using **example@gmail.com**
- JWT identity verification using `/rest/user/whoami`
- Administrative endpoint discovery from Target Site Map
- Authorization testing of `/rest/admin/application-configuration`

---

# Conclusion

Authorization testing was performed against multiple authenticated API endpoints using Burp Suite. JWT tokens from different user accounts were used to verify identity handling and authorization behavior.

The `/rest/user/whoami` endpoint correctly enforced authentication by requiring valid session credentials before returning user information.

The `/rest/admin/application-configuration` endpoint returned application configuration data regardless of authentication state during testing. This behavior was documented as part of the assessment because it exposes configuration information that may aid reconnaissance.

Overall, the assessment demonstrated the use of manual authorization testing techniques including JWT verification, token manipulation, authentication validation, and endpoint access analysis using Burp Suite.

# 15_Login_Request_Testing

## Objective
Evaluate the authentication mechanism of the OWASP Juice Shop login functionality by performing manual request manipulation using Burp Suite to identify authentication flaws, improper input validation, error handling weaknesses, and SQL Injection vulnerabilities.

---

## Burp Suite Tool Used
- **Proxy** – Captured the original login request.
- **Repeater** – Modified and replayed authentication requests for manual security testing.

---

## Target Endpoint

```
POST /rest/user/login
```

---

## Security Tests Performed

### 1. Valid Authentication
- Sent original login credentials.
- Verified successful authentication.
- Server returned **HTTP 200 OK** with a JWT authentication token.

---

### 2. Invalid Credential Testing
- Modified username and password.
- Verified server response for invalid credentials.
- Server correctly returned:

```
HTTP/1.1 401 Unauthorized
```

---

### 3. Input Validation Testing
Tested multiple malformed inputs including:

- Empty username
- Empty password
- Null values
- Long strings
- Random characters
- Special characters

Observed that invalid requests were rejected without authentication.

---

### 4. SQL Injection Authentication Testing
Injected SQL payloads into the login request.

Example payload:

```sql
' OR 1=1--
```

The server unexpectedly authenticated the request and returned:

```
HTTP/1.1 200 OK
```

along with a valid authentication token.

**Finding**

Authentication bypass through SQL Injection was successfully demonstrated.

---

### 5. Content-Type Manipulation
Modified the request header:

```
Content-Type: application/json
```

to alternative values such as

```
text/plain
application/xml
multipart/form-data
```

The application rejected malformed requests and returned authentication failures, indicating proper request format validation.

---

### 6. Malformed JSON Testing
Sent intentionally malformed JSON payloads.

Example:

```json
{"email":"test@gmail.com","password":"Test@123"
```

The application generated an internal server error and disclosed detailed parser information.

Observed response:

```
HTTP/1.1 500 Internal Server Error
```

with JSON parsing stack trace.

---

## Key Findings

| Test | Result |
|-------|--------|
| Valid Login | ✅ Successful |
| Invalid Credentials | ✅ Properly Rejected |
| Empty Parameters | ✅ Properly Validated |
| Input Validation | ✅ Implemented |
| Content-Type Validation | ✅ Implemented |
| Malformed JSON | ⚠ Verbose Error Disclosure |
| SQL Injection Authentication Bypass | 🚨 Vulnerable |

---

## Security Impact

### High Severity
- SQL Injection authentication bypass allows unauthorized access without valid credentials.

### Medium Severity
- Verbose error messages disclose backend implementation details and stack traces that could assist attackers during reconnaissance.

---

## Evidence Captured

- Original Login Request
- Successful Authentication (HTTP 200)
- Invalid Login Response (HTTP 401)
- SQL Injection Authentication Bypass
- Verbose Error Disclosure
- Header Manipulation Testing

---

## Conclusion

Manual authentication testing demonstrated that the login functionality correctly validates most malformed requests and invalid credentials. However, SQL Injection testing revealed a critical authentication bypass vulnerability, allowing unauthorized access. Additionally, malformed JSON requests exposed verbose backend error messages, indicating insufficient exception handling. These findings highlight the importance of parameterized database queries, secure error handling, and comprehensive input validation to strengthen the authentication mechanism.

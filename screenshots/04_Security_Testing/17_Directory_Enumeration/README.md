# Directory Enumeration

## Overview

Directory Enumeration was performed against the OWASP Juice Shop application using **Burp Suite Professional Intruder** to identify accessible directories, REST endpoints, API routes, administrative resources, and hidden application functionality.

The objective of this assessment was to discover publicly accessible resources, identify protected endpoints, observe server response behavior, and understand the application's attack surface.

---

## Objective

- Discover accessible directories and API endpoints.
- Identify hidden or undocumented application resources.
- Detect administrative and sensitive endpoints.
- Differentiate between public and protected resources.
- Analyze HTTP response codes and response lengths.
- Expand the application's attack surface for further security testing.

---

## Testing Methodology

Burp Suite Intruder was configured in **Sniper Attack** mode.

A single payload position was inserted into different URL paths while a custom endpoint wordlist was used to enumerate possible application resources.

Example payloads included:

- admin
- api
- rest
- login
- users
- products
- basket
- profile
- assets
- uploads
- robots.txt
- swagger
- configuration
- administration

Each request was analyzed based on:

- HTTP Status Code
- Response Length
- Response Time
- Response Headers
- Authorization Requirements
- Resource Accessibility

---

# Enumeration Scenarios

---

## 1. Root Endpoint Enumeration

**Request Pattern**

```
GET /<payload>
```

### Purpose

Enumerate directories directly under the application's root path.

### Observations

- Public resources returned **HTTP 200 OK**
- Non-existing routes generated **HTTP 500**
- Static resources redirected using **HTTP 301**
- Multiple application endpoints were successfully discovered.

### Screenshot

**15_Intruder_Root_Endpoint_Enumeration.png**

---

## 2. API Endpoint Enumeration

**Request Pattern**

```
GET /api/<payload>/
```

### Purpose

Identify available API routes and verify authorization enforcement.

### Observations

- Protected endpoints returned **HTTP 401 Unauthorized**
- Public API resources returned **HTTP 200 OK**
- Invalid API routes generated server errors
- Authentication was properly enforced for sensitive resources.

### Screenshot

**16_Intruder_API_Endpoint_Enumeration.png**

---

## 3. REST Endpoint Enumeration

**Request Pattern**

```
GET /rest/<payload>
```

### Purpose

Discover REST-based application endpoints and identify protected services.

### Observations

- Several REST endpoints required authentication.
- Unauthorized resources returned **HTTP 401 Unauthorized**.
- Invalid endpoints generated **HTTP 500 Internal Server Error**.
- Public resources remained accessible without authentication.

### Screenshot

**17_Intruder_REST_Endpoint_Enumeration.png**

---

## 4. Profile Endpoint Enumeration

**Request Pattern**

```
GET /profile/<payload>
```

### Purpose

Enumerate profile-related resources and verify access control implementation.

### Observations

- Public profile resources returned **HTTP 200 OK**
- Sensitive profile endpoints were identified
- Response length comparison helped distinguish valid resources
- No unintended information disclosure was observed.

### Screenshot

**18_Intruder_Profile_Endpoint_Enumeration.png**

---

# Response Analysis

During enumeration, the following response codes were observed:

| Status Code | Meaning | Observation |
|-------------|---------|-------------|
| **200 OK** | Resource Accessible | Public endpoint successfully reached |
| **301 Moved Permanently** | Redirect | Static resources redirected correctly |
| **401 Unauthorized** | Authentication Required | Access control enforced |
| **500 Internal Server Error** | Invalid/Internal Processing | Invalid or unsupported endpoint |

---

# Security Observations

- Public resources were correctly accessible.
- Protected API endpoints required authentication.
- Administrative resources were not directly exposed.
- REST endpoints enforced authorization where expected.
- Enumeration revealed multiple application routes suitable for further assessment.
- Response length variations helped distinguish valid endpoints from invalid ones.
- No directory listing or sensitive file exposure was identified during enumeration.

---

# Tools Used

- Burp Suite Professional
- Burp Intruder
- HTTP History
- Response Inspector
- OWASP Juice Shop
- Docker

---

# Conclusion

Directory Enumeration successfully identified the application's publicly accessible resources, REST services, API endpoints, and protected functionality.

The testing confirmed that the application properly enforced authentication on sensitive resources while exposing only intended public endpoints. The discovered routes provided valuable insight into the application's structure and established a solid foundation for subsequent testing phases, including Authorization Testing, Parameter Manipulation, Authentication Testing, and API Security Assessment.

# OWASP Juice Shop Web Application Security Assessment Report

## Project Overview

This project presents a comprehensive Web Application Security Assessment performed against the intentionally vulnerable **OWASP Juice Shop** application using **Burp Suite Professional**. The assessment was conducted within a controlled laboratory environment to evaluate the application's security posture by identifying authentication weaknesses, authorization issues, access control vulnerabilities, input validation flaws, business logic weaknesses, information disclosure, and improper server-side request handling.

The assessment followed a structured manual penetration testing methodology aligned with the **OWASP Web Security Testing Guide (WSTG)** and referenced the **OWASP Top 10 (2021)** security risks where applicable.

Throughout the assessment, multiple application functionalities including authentication, authorization, directory enumeration, parameter manipulation, checkout processing, shopping basket management, address management, payment card handling, customer feedback, and search functionality were evaluated through manual HTTP request analysis and manipulation.

> **Disclaimer:** This assessment was performed exclusively against OWASP Juice Shop within an isolated laboratory environment for educational and security research purposes. No unauthorized systems were tested.

---

# Assessment Objectives

The primary objectives of this assessment were to:

- Deploy OWASP Juice Shop using Docker.
- Configure Burp Suite Professional as an intercepting proxy.
- Capture and analyze HTTP requests and responses.
- Map the application's exposed attack surface.
- Perform authentication and authorization testing.
- Identify access control weaknesses.
- Evaluate endpoint security.
- Assess server-side input validation.
- Perform parameter manipulation across critical application endpoints.
- Validate business logic implementation.
- Identify information disclosure vulnerabilities.
- Document security findings with supporting evidence.
- Recommend remediation measures based on industry best practices.

---

# Lab Environment

| Component | Details |
|------------|--------------------------------|
| Host Operating System | Windows 11 |
| Target Application | OWASP Juice Shop |
| Deployment Method | Docker Container |
| Docker Image | bkimminich/juice-shop |
| Web Proxy | Burp Suite Professional |
| Browser | Burp Suite Built-in Browser |
| Local URL | http://localhost:3000 |
| Testing Method | Manual Web Application Penetration Testing |

---

# Tools Used

- Burp Suite Professional
- Docker Desktop
- OWASP Juice Shop
- Burp Suite Built-in Browser
- Burp Proxy
- Burp Target
- Burp Repeater
- Burp Intruder
- Burp Decoder
- Burp Comparer

---

# Assessment Results

The assessment successfully demonstrated:

- Successful interception and analysis of HTTP requests.
- Comprehensive application mapping.
- Manual authorization assessment.
- Directory enumeration of exposed API endpoints.
- Authentication and session analysis.
- Manual parameter manipulation across multiple application endpoints.
- Identification of access control weaknesses.
- Discovery of information disclosure issues.
- Evaluation of server-side validation mechanisms.
- Business logic assessment.
- Functional verification of application workflows.

---

# Executive Findings Summary

The assessment identified several security weaknesses affecting access control, input validation, server-side error handling, and business logic implementation.

| Finding ID | Finding | Severity |
|------------|----------------------------------------------|-----------|
| F-001 | Public Application Configuration Disclosure | 🟡 Medium |
| F-002 | Insecure Direct Object Reference (Checkout Basket ID) | 🟠 High |
| F-003 | Verbose Error Message Disclosure | 🟡 Medium |
| F-004 | Inconsistent Server-Side Input Validation | 🟡 Medium |
| F-005 | Missing Mandatory Parameter Validation | 🟡 Medium |
| F-006 | Authentication Context Inconsistency | 🟡 Medium |
| F-007 | Duplicate Business Object Creation | 🔵 Low |
| F-008 | Search Endpoint SQL Error Disclosure | 🟡 Medium |

---

# Risk Rating Methodology

Risk ratings were assigned based on the potential impact on confidentiality, integrity, availability, exploitability, and business risk.

| Severity | Description |
|-----------|-------------|
| 🔴 Critical | Immediate compromise of application or sensitive data. |
| 🟠 High | Significant security weakness requiring immediate remediation. |
| 🟡 Medium | Moderate security issue that could assist attackers or weaken security posture. |
| 🔵 Low | Minor security weakness with limited impact. |
| ⚪ Informational | Observation or best practice recommendation. |

---

# Detailed Findings

## F-001 – Public Application Configuration Disclosure

**Severity:** 🟡 Medium

**Affected Endpoint**

```http
GET /rest/admin/application-configuration
```

### Description

During authorization testing, the administrative configuration endpoint was directly accessible without requiring authenticated administrative privileges. The endpoint consistently returned application configuration information, including feature settings, deployment metadata, and configuration values.

### Observation

The endpoint returned:

```http
HTTP/1.1 200 OK
```

under multiple authentication scenarios, including requests without valid authentication credentials.

The response disclosed internal application configuration information that could assist an attacker during reconnaissance.

### Impact

Although no direct privilege escalation was observed, publicly accessible configuration information may reveal implementation details useful for planning further attacks.

**OWASP Top 10 (2021)**

A05 – Security Misconfiguration

**CWE**

CWE-200 – Exposure of Sensitive Information to an Unauthorized Actor

**Remediation**

- Restrict access to administrative configuration endpoints.
- Implement role-based authorization.
- Avoid exposing internal configuration values through publicly accessible APIs.

---

## F-002 – Insecure Direct Object Reference (IDOR)

**Severity:** 🟠 High

**Affected Endpoint**

```http
POST /api/Orders/
```

### Description

During checkout testing, the `basketId` parameter was manually modified to reference another authenticated user's shopping basket while maintaining the attacker's own authentication credentials.

The application successfully processed the modified request and generated an order using the targeted basket without validating ownership of the supplied basket identifier.

### Observation

The modified request returned:

```http
HTTP/1.1 200 OK
```

The response contained products belonging to the referenced basket.

The targeted user's basket became empty after checkout, while the generated order appeared in the attacker's order history.

This behavior indicates insufficient server-side authorization checks on user-controlled resource identifiers.

### Impact

An authenticated attacker may manipulate resource identifiers to perform unauthorized actions on resources belonging to other users.

**OWASP Top 10 (2021)**

A01 – Broken Access Control

**CWE**

CWE-639 – Authorization Bypass Through User-Controlled Key

**Remediation**

- Validate basket ownership on the server.
- Never trust client-supplied resource identifiers.
- Associate basket resources using the authenticated user session.
- Return **HTTP 403 Forbidden** for unauthorized access attempts.

---

## F-003 – Verbose Error Message Disclosure

**Severity:** 🟡 Medium

### Description

Several malformed requests generated verbose internal server errors revealing implementation details.

Observed information included:

- SQLite database errors
- Express framework stack traces
- JavaScript parser exceptions
- SQL query information

### Observation

Malformed requests frequently generated:

```http
HTTP/1.1 500 Internal Server Error
```

instead of generic application error messages.

### Impact

Verbose error messages provide attackers with valuable reconnaissance information regarding the application's internal implementation.

**OWASP Top 10 (2021)**

A05 – Security Misconfiguration

**CWE**

CWE-209 – Information Exposure Through an Error Message

**Remediation**

- Return generic error responses.
- Disable stack traces in production.
- Log detailed exceptions only within server logs.

---

## F-004 – Inconsistent Server-Side Input Validation

**Severity:** 🟡 Medium

### Description

Multiple application endpoints demonstrated inconsistent validation of user-supplied parameters.

Testing identified scenarios where malformed input generated successful responses, client errors, or internal server errors depending on the affected endpoint.

### Observation

Examples included:

- Missing mandatory parameters accepted.
- Invalid datatypes generated HTTP 500.
- Arbitrary numeric values accepted.
- Negative values accepted for certain parameters.
- Some malformed input produced HTTP 201.

### Impact

Inconsistent validation increases application complexity and may expose additional attack vectors.

**OWASP Top 10 (2021)**

A03 – Injection

**CWE**

CWE-20 – Improper Input Validation

**Remediation**

- Implement centralized server-side validation.
- Validate datatype, length, range and format.
- Reject malformed requests consistently using HTTP 400.

---

## F-005 – Missing Mandatory Parameter Validation

**Severity:** 🟡 Medium

### Description

Several mandatory parameters could be completely removed from requests while the application still processed the request successfully.

Examples included:

- Card Number
- Expiration Month
- Expiration Year
- Full Name
- Mobile Number
- ZIP Code
- UserId

### Observation

Removing mandatory parameters often resulted in:

```http
HTTP/1.1 201 Created
```

with null values stored by the application.

### Impact

Improper business validation may reduce data integrity and introduce unexpected application behavior.

**OWASP Top 10 (2021)**

A04 – Insecure Design

**CWE**

CWE-20 – Improper Input Validation

**Remediation**

- Enforce mandatory server-side validation.
- Reject incomplete requests.
- Apply database constraints where appropriate.

---

## F-006 – Authentication Context Inconsistency

**Severity:** 🟡 Medium

### Description

Authentication handling differed between endpoints.

Some endpoints primarily relied on the Authorization Bearer token while others relied on the authentication cookie to determine user identity.

### Observation

Examples observed:

- `/rest/user/whoami` relied primarily on the authentication cookie.
- Address creation relied primarily on the Authorization Bearer token.
- Card creation relied primarily on the Authorization Bearer token.

### Impact

Inconsistent authentication mechanisms may complicate authorization logic and increase the likelihood of access control weaknesses.

**OWASP Top 10 (2021)**

A01 – Broken Access Control

**CWE**

CWE-287 – Improper Authentication

**Remediation**

- Standardize authentication across all endpoints.
- Perform authorization using a single trusted authentication context.

---

## F-007 – Duplicate Business Object Creation

**Severity:** 🔵 Low

### Description

The application allowed duplicate address and payment card creation without enforcing uniqueness.

### Observation

Repeated requests generated:

```http
HTTP/1.1 201 Created
```

creating duplicate resources successfully.

### Impact

Although primarily a business logic issue, duplicate resource creation may affect data quality and operational consistency.

**OWASP Top 10 (2021)**

A04 – Insecure Design

**CWE**

CWE-840 – Business Logic Errors

**Remediation**

- Prevent duplicate records where appropriate.
- Apply uniqueness constraints.
- Validate existing resources before creation.

---

## F-008 – Search Endpoint SQL Error Disclosure

**Severity:** 🟡 Medium

### Description

Malformed SQL injection payloads generated verbose SQLite database errors.

### Observation

Payload:

```text
'--
```

generated:

```http
HTTP/1.1 500 Internal Server Error
```

The response exposed:

- SQL query
- SQLite error
- Stack trace
- Database implementation details

### Impact

Although SQL Injection was not confirmed, detailed database errors assist attackers during reconnaissance.

**OWASP Top 10 (2021)**

A05 – Security Misconfiguration

**CWE**

CWE-209 – Information Exposure Through an Error Message

**Remediation**

- Hide database errors.
- Return generic error responses.
- Implement centralized exception handling.

---

# Results

The assessment successfully demonstrated:

- HTTP request interception and modification.
- Application endpoint enumeration.
- Authentication workflow analysis.
- Authorization testing.
- Identification of an IDOR vulnerability.
- Parameter manipulation across critical endpoints.
- Business logic validation.
- Input validation assessment.
- Functional verification of application features.
- Documentation of verified security findings.

---

# Skills Demonstrated

- Web Application Penetration Testing
- Burp Suite Professional
- HTTP Request Analysis
- REST API Testing
- Authentication Testing
- Authorization Testing
- IDOR Assessment
- Parameter Manipulation
- Business Logic Testing
- Input Validation Testing
- Error Handling Assessment
- Manual Security Testing
- Professional Security Documentation

---

# OWASP Top 10 (2021) Mapping

| Finding | OWASP Category |
|----------|----------------|
| IDOR | A01 – Broken Access Control |
| Authentication Inconsistency | A01 – Broken Access Control |
| Input Validation Weaknesses | A03 – Injection |
| Missing Mandatory Validation | A04 – Insecure Design |
| Duplicate Object Creation | A04 – Insecure Design |
| Configuration Disclosure | A05 – Security Misconfiguration |
| Verbose Error Messages | A05 – Security Misconfiguration |
| SQL Error Disclosure | A05 – Security Misconfiguration |

---

# CWE Mapping

| Finding | CWE |
|----------|------|
| IDOR | CWE-639 |
| Configuration Disclosure | CWE-200 |
| Verbose Error Messages | CWE-209 |
| SQL Error Disclosure | CWE-209 |
| Input Validation | CWE-20 |
| Missing Mandatory Validation | CWE-20 |
| Authentication Inconsistency | CWE-287 |
| Duplicate Resource Creation | CWE-840 |

---

# Risk Matrix

| Severity | Findings |
|-----------|----------|
| 🔴 Critical | None |
| 🟠 High | 1 |
| 🟡 Medium | 6 |
| 🔵 Low | 1 |
| ⚪ Informational | 0 |

---

# Security Recommendations

Based on the assessment findings, the following recommendations are advised:

- Implement robust server-side authorization checks for all user-controlled resource identifiers.
- Standardize authentication validation across all API endpoints.
- Enforce comprehensive server-side input validation for mandatory parameters.
- Prevent verbose error messages from exposing internal implementation details.
- Restrict access to administrative and configuration endpoints.
- Validate business rules before processing requests.
- Prevent duplicate resource creation where appropriate.
- Apply consistent HTTP response handling for malformed requests.
- Perform regular security assessments following the OWASP Web Security Testing Guide.
- Continuously monitor application logs for unauthorized or suspicious activity.

---

# Key Learning Outcomes

This assessment provided practical experience in manual web application penetration testing using Burp Suite Professional. It strengthened understanding of authentication mechanisms, authorization controls, REST API security, business logic testing, parameter manipulation, IDOR identification, and server-side input validation.

The assessment also reinforced the importance of secure error handling, proper access control implementation, and comprehensive validation of user-controlled input in modern web applications.

---

# Conclusion

The OWASP Juice Shop Web Application Security Assessment successfully demonstrated a structured manual penetration testing methodology against a modern web application.

Multiple application components were evaluated, including authentication, authorization, endpoint exposure, parameter handling, business logic, and functional workflows.

The assessment identified several security weaknesses, including an **Insecure Direct Object Reference (IDOR)** vulnerability, information disclosure issues, inconsistent input validation, authentication inconsistencies, and business logic weaknesses. While these findings are intentionally present within OWASP Juice Shop for educational purposes, they accurately represent common vulnerabilities encountered during real-world web application security assessments.

Overall, the project demonstrates practical proficiency in manual web application penetration testing, HTTP request analysis, Burp Suite Professional, REST API assessment, vulnerability verification, and professional security documentation aligned with industry best practices.

---

# References

- OWASP Web Security Testing Guide (WSTG)
- OWASP Top 10 (2021)
- OWASP Juice Shop Documentation
- Burp Suite Professional Documentation
- MITRE CWE Catalog
- Docker Documentation

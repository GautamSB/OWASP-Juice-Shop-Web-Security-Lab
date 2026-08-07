# Security Testing

## Objective

Perform a comprehensive manual security assessment of the OWASP Juice Shop web application by identifying and evaluating authentication, authorization, input validation, access control, business logic, and server-side validation weaknesses. The assessment was conducted using Burp Suite Professional to analyze application behavior, manipulate HTTP requests, and verify security controls across multiple API endpoints. OWASP Juice Shop is intentionally designed as a vulnerable application for security training and assessment.

---

# Assessment Scope

The following security assessments were performed during the engagement:

* Authentication Testing
* Authorization Testing
* Directory Enumeration
* Parameter Manipulation
* Search Parameter Testing
* Basket Parameter Testing
* Address Parameter Testing
* Card Parameter Testing
* Checkout Parameter Testing
* Feedback Parameter Testing

---

# Burp Suite Tools Used

* **Proxy** – Captured and intercepted HTTP requests and responses.
* **Target Site Map** – Mapped application endpoints and resources.
* **Repeater** – Replayed and modified requests to evaluate server-side validation.
* **Intruder** – Automated payload injection for testing input validation, fuzzing parameters, and identifying potential injection points.
* **HTTP History** – Reviewed application traffic throughout testing.

---

# Security Testing Methodology

## 1. Authentication Testing

Authentication controls were evaluated by testing user registration, login functionality, JWT generation, session handling, and authentication validation.

Performed activities:

* User registration
* User login
* JWT acquisition
* Session validation
* Authentication request analysis

---

## 2. Authorization Testing

Authorization mechanisms were assessed by modifying authentication credentials, JWT tokens, cookies, and protected API requests.

Performed activities:

* JWT validation
* Cookie manipulation
* Authorization header manipulation
* User identity verification
* Administrative endpoint testing
* Insecure Direct Object Reference (IDOR) verification

---

## 3. Directory Enumeration

Application mapping was performed to identify accessible endpoints and resources exposed by the application.

Performed activities:

* Endpoint discovery
* API enumeration
* Resource identification
* Target Site Map analysis

---

## 4. Parameter Manipulation

Server-side validation was evaluated by modifying request parameters before submission.

Endpoints assessed included:

* Basket
* Address
* Payment Card
* Checkout
* Feedback
* Product Search

Testing included:

* Missing parameters
* Null values
* Invalid data types
* Numeric boundary testing
* Negative values
* Long input values
* SQL Injection payloads
* Cross-Site Scripting (XSS) payloads
* Authentication parameter manipulation
* Business logic validation

---

## 5. Search Parameter Testing

The product search functionality was assessed by manipulating the search query parameter.

Testing included:

* Empty search values
* SQL Injection payloads
* XSS payloads
* URL-encoded input
* Wildcard characters
* Special characters

---

## 6. Input Validation Assessment

User-controlled input fields across multiple application features were evaluated for server-side validation.

Parameters assessed included:

* Address fields
* Payment card information
* Basket identifiers
* Checkout parameters
* Feedback fields
* Search queries

---

## 7. Business Logic Testing

Business logic controls were evaluated by manipulating valid application workflows.

Observed scenarios included:

* Authentication binding verification
* Duplicate address creation
* Duplicate payment card creation
* Basket ownership validation
* Checkout workflow validation
* Order generation behavior

---

## 8. Error Handling Assessment

Malformed requests and invalid parameter values were submitted to evaluate server-side error handling.

Observed responses included:

* HTTP 400 Bad Request
* HTTP 401 Unauthorized
* HTTP 500 Internal Server Error

Responses were reviewed for unnecessary disclosure of internal implementation details.

---

# Assessment Coverage

| Security Area                      | Status      |
| ---------------------------------- | ----------- |
| Authentication Testing             | ✅ Completed |
| Authorization Testing              | ✅ Completed |
| JWT Validation                     | ✅ Completed |
| Cookie Manipulation                | ✅ Completed |
| IDOR Verification                  | ✅ Completed |
| Directory Enumeration              | ✅ Completed |
| Parameter Manipulation             | ✅ Completed |
| Search Parameter Testing           | ✅ Completed |
| Basket Parameter Testing           | ✅ Completed |
| Address Parameter Testing          | ✅ Completed |
| Card Parameter Testing             | ✅ Completed |
| Checkout Parameter Testing         | ✅ Completed |
| Feedback Parameter Testing         | ✅ Completed |
| SQL Injection Testing              | ✅ Completed |
| Cross-Site Scripting (XSS) Testing | ✅ Completed |
| Input Validation Assessment        | ✅ Completed |
| Business Logic Testing             | ✅ Completed |
| Error Handling Assessment          | ✅ Completed |
| Information Disclosure Assessment  | ✅ Completed |

---

# Security Observations

The assessment identified several behaviors related to server-side validation, business logic enforcement, authentication handling, authorization controls, and error handling.

Observed behaviors included:

* Inconsistent validation of mandatory parameters.
* Acceptance of duplicate business objects such as addresses and payment cards.
* Authentication behavior varying between Authorization headers and authentication cookies depending on the endpoint.
* Verbose internal error messages under certain malformed request conditions.
* Publicly accessible configuration information on specific endpoints.
* Successful handling of many SQL Injection and XSS payloads without evidence of exploitation, while some malformed input generated internal server errors.

All observations were documented within the corresponding assessment reports.

---

# Evidence Collected

The assessment documentation includes supporting evidence for:

* HTTP request and response analysis
* Authentication testing
* Authorization testing
* Endpoint discovery
* Parameter manipulation
* SQL Injection testing
* Cross-Site Scripting testing
* Business logic validation
* Error handling analysis
* Functional verification

---

# Conclusion

A comprehensive manual security assessment was conducted against the OWASP Juice Shop application using Burp Suite Professional.

The assessment covered authentication, authorization, endpoint enumeration, parameter manipulation, search functionality, server-side input validation, business logic validation, and error handling across multiple application components.

The testing methodology demonstrated practical web application security testing techniques, including HTTP request manipulation, JWT analysis, authorization verification, endpoint discovery, input validation assessment, and business logic evaluation.

Each assessment area is documented separately within this repository, providing detailed methodologies, observations, supporting evidence, and conclusions for the corresponding application functionality.

# Parameter Manipulation Testing

## Objective

Evaluate the application's server-side validation, business logic enforcement, authentication handling, authorization checks, and error handling by manipulating user-controlled request parameters across multiple API endpoints.

The assessment focused on identifying weaknesses related to:

- Server-side input validation
- Business logic validation
- Parameter tampering
- Authentication handling
- Error handling
- Information disclosure
- SQL Injection handling
- Cross-Site Scripting (XSS) input handling
- Object identifier validation

---

# Scope

Parameter manipulation testing was performed against the following application functionalities:

- Product Search
- Basket
- Checkout
- Address Management
- Payment Card Management
- Feedback Submission

---

# Burp Suite Tools Used

- **Proxy** – Captured HTTP requests and responses.
- **Target Site Map** – Identified API endpoints.
- **Repeater** – Modified request parameters and replayed requests for manual validation.

---

# Endpoints Tested

| Functionality | Endpoint |
|--------------|----------|
| Search | `GET /rest/products/search?q=` |
| Basket | `POST /api/BasketItems/` |
| Checkout | `POST /rest/basket/{basketId}/checkout` |
| Address | `POST /api/Addresss/` |
| Payment Card | `POST /api/Cards/` |
| Feedback | `POST /api/Feedbacks/` |

---

# Testing Methodology

The following parameter manipulation techniques were applied throughout the assessment.

## Authentication Manipulation

Authentication tokens were modified to determine how the application binds authenticated requests to user resources.

Testing included:

- Authorization Bearer Token
- Authentication Cookie
- Mixed authentication tokens between different user accounts
- Missing authentication headers

---

## Parameter Validation

Application parameters were modified using multiple input variations including:

- Empty values
- Null values
- Missing parameters
- Long input
- Short input
- Numeric values
- Negative values
- Alphabetic values
- Alphanumeric values
- Special characters

---

## SQL Injection Testing

SQL injection payloads were submitted through user-controlled parameters including:

```text
'
'--
' OR 1=1--
```

The objective was to determine whether server-side queries could be manipulated through crafted input.

---

## Cross-Site Scripting (XSS)

Multiple XSS payloads were submitted through user-controlled parameters including:

```html
<script>alert(1)</script>

<img src=x onerror=alert(1)>
```

Testing verified whether the application reflected, stored, filtered, or executed malicious HTML and JavaScript.

---

## Business Logic Validation

Business rules were evaluated by intentionally supplying unexpected parameter values such as:

- Missing mandatory fields
- Invalid identifiers
- Duplicate requests
- Unexpected numeric values
- Invalid object references

---

## Error Handling Assessment

Malformed requests were intentionally generated to evaluate server-side exception handling.

Testing included:

- Malformed JSON
- Invalid object identifiers
- Invalid parameter types

---

# Summary of Key Findings

| Assessment Area | Observation |
|-----------------|-------------|
| Authentication Handling | Different endpoints relied on different authentication mechanisms (Authorization header and Cookie). |
| Server-Side Validation | Validation was inconsistent across multiple endpoints. |
| SQL Injection | SQL injection payloads were generally handled safely, although one malformed payload triggered verbose database errors. |
| Cross-Site Scripting | Submitted XSS payloads were accepted or stored in some locations but no client-side execution was observed during testing. |
| Business Logic Validation | Several endpoints accepted missing or unexpected parameter values while still processing requests successfully. |
| Error Handling | Invalid requests frequently generated HTTP 500 responses exposing internal implementation details and stack traces. |
| Information Disclosure | Internal SQLite errors, parser exceptions, and application stack traces were disclosed under specific error conditions. |

---

# Security Observations

## Server-Side Validation

Testing identified inconsistent validation across different API endpoints.

Several endpoints accepted:

- Missing parameters
- Null values
- Unexpected input formats
- Invalid business values

while continuing normal request processing.

---

## Business Logic Validation

Business rule enforcement varied between endpoints.

Examples observed during testing included:

- Resources created with missing mandatory fields.
- Duplicate operations accepted.
- Checkout processing continued despite missing request parameters.
- Authentication behavior differed depending on the endpoint.

---

## SQL Injection Handling

Multiple SQL injection payloads were submitted across different endpoints.

No successful SQL injection was identified during testing.

However, malformed SQL payloads submitted to the search functionality generated verbose SQLite database errors that disclosed backend implementation details.

---

## Cross-Site Scripting Handling

Various stored and reflected XSS payloads were submitted through user-controlled parameters.

Although several payloads were accepted as ordinary input, no JavaScript execution was observed during testing.

---

## Error Handling

Malformed requests frequently generated:

```http
HTTP/1.1 500 Internal Server Error
```

Several responses exposed:

- SQLite error messages
- JavaScript exceptions
- Internal stack traces
- Parser exceptions
- Application file paths

These responses reveal implementation details that could assist attackers during reconnaissance.

---

# Repository Structure

```text
Parameter_Manipulation_Testing/
│
├── Basket_Parameter_Manipulation/
├── Checkout_Parameter_Manipulation/
├── Address_Parameter_Manipulation/
├── Card_Parameter_Manipulation/
├── Feedback_Parameter_Manipulation/
├── Search_Parameter_Manipulation/
│
└── README.md
```

---

# Evidence Captured

Evidence was collected for each tested endpoint, including:

- Original requests
- Modified requests
- HTTP responses
- Error messages
- Validation behaviour
- Authentication behaviour
- Business logic observations

Detailed findings for each endpoint are documented within their respective directories.

---

# Conclusion

Parameter manipulation testing was performed across multiple high-value API endpoints using Burp Suite Repeater to evaluate server-side validation, business logic enforcement, authentication handling, and error management.

The assessment demonstrated that while the application correctly handled many malformed inputs without resulting in successful exploitation, several inconsistencies were identified in parameter validation, business rule enforcement, and exception handling. Multiple endpoints generated verbose internal error messages that disclosed implementation details, while others accepted unexpected or incomplete input without enforcing strict validation.

Overall, the assessment demonstrates a comprehensive manual evaluation of parameter manipulation techniques across the application's core functionality and provides endpoint-specific evidence for each identified behaviour within the corresponding testing directories.

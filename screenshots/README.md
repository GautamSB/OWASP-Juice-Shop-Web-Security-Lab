# Screenshots

## Objective

This directory contains screenshots captured during the **OWASP Juice Shop Web Application Security Assessment** performed using **Burp Suite Professional**. The screenshots provide visual evidence of the assessment methodology, application mapping, HTTP request and response analysis, manual security testing, Burp Suite tool demonstrations, and functional verification of the application's core features.

The evidence presented in this directory supports the observations, findings, and conclusions documented throughout the assessment reports contained within this repository.

---

# Screenshot Categories

The screenshots are organized into the following categories:

- Environment Setup
- Application Mapping
- HTTP Request & Response Analysis
- Security Testing
- Burp Suite Tool Demonstration
- Functional Testing

---

# Environment Setup

| Screenshot | Description |
|------------|-------------|
| 01_Docker_Container_Running.png | OWASP Juice Shop Docker container running successfully |
| 02_Burp_Proxy_Listener.png | Burp Suite Professional proxy listener configuration |
| 03_Burp_Browser_JuiceShop_Homepage.png | OWASP Juice Shop application loaded within Burp Suite Browser |

---

# Application Mapping

| Screenshot | Description |
|------------|-------------|
| 04_HTTP_History.png | HTTP History displaying captured application traffic |
| 05_Target_Site_Map.png | Burp Suite Target Site Map showing discovered application endpoints |

---

# HTTP Request & Response Analysis

| Screenshot | Description |
|------------|-------------|
| 06_Login_Request_Response.png | User authentication request and response |
| 07_Address_Request_Response.png | Address creation request and response |
| 08_Card_Request_Response.png | Payment card creation request and response |
| 09_Delivery_Request_Response.png | Delivery method request and response |
| 10_Add_To_Basket_Request_Response.png | Product added to basket request and response |
| 11_Checkout_Request_Response.png | Checkout request and response |
| 12_Feedback_Request_Response.png | Customer feedback submission request and response |
| 13_Complaint_Request_Response.png | Complaint submission request and response |
| 14_User_Registration_Request_Response.png | User registration request and response |

---

# Security Testing

| Screenshot | Description |
|------------|-------------|
| 15_Login_Request_Testing.png | Authentication testing and login request validation |
| 16_Authorization_Testing.png | Authorization assessment including JWT validation, cookie verification, administrative endpoint testing, and IDOR verification |
| 17_Directory_Enumeration.png | Application endpoint discovery using Burp Suite |
| 18_Parameter_Manipulation.png | Parameter manipulation assessment across Basket, Address, Card, Checkout, Feedback, and Search API endpoints |

---

# Functional Testing

| Screenshot | Description |
|------------|-------------|
| 19_Product_Search.png | Product search functionality verification |
| 20_Basket_Management.png | Basket operations including add, update quantity, remove item, and empty basket |
| 21_Address_Management.png | Address creation and management functionality |
| 22_Card_Management.png | Payment card management functionality |
| 23_Checkout_Success.png | Successful checkout confirmation |
| 24_Order_History.png | Verification of completed orders in Order History |
| 25_Order_Tracking.png | Order tracking functionality verification |
| 26_Feedback_Submission.png | Customer feedback submission |
| 27_Complaint_Submission.png | Customer complaint submission |

---

# Decoder and Comparer

| Screenshot | Description |
|------------|-------------|
| 28_JWT_Decoding.png | JWT token decoding and payload inspection using Burp Suite Decoder |
| 29_URL_Base64_Encoding_Decoding.png | URL encoding/decoding and Base64 encoding/decoding using Burp Suite Decoder |
| 30_JWT_Response_Comparison.png | Comparison of authentication responses for two user accounts using Burp Suite Comparer |
| 31_Address_Response_Comparison.png | Comparison of address creation responses highlighting differences in HTTP headers and JSON data using Burp Suite Comparer |

---

# Assessment Coverage

The screenshots provide supporting evidence for the following assessment activities:

- Environment configuration
- Application mapping and endpoint discovery
- HTTP request and response analysis
- Authentication testing
- Authorization testing
- Directory enumeration
- Parameter manipulation
- Search parameter testing
- Input validation assessment
- SQL Injection testing
- Cross-Site Scripting (XSS) testing
- Business logic testing
- Insecure Direct Object Reference (IDOR) verification
- Error handling assessment
- JWT decoding and authentication token analysis
- URL and Base64 encoding/decoding
- HTTP request and response comparison
- Functional verification of core application features

---

# Notes

All screenshots were captured during manual testing performed against the **OWASP Juice Shop** application running within a controlled laboratory environment using **Burp Suite Professional**.

The screenshots are intended solely as supporting evidence for the observations, findings, and conclusions documented throughout this repository. Each image corresponds to a specific phase of the assessment and demonstrates the practical application of manual web application security testing techniques and Burp Suite analysis capabilities.

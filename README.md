# OWASP Juice Shop Web Application Security Assessment using Burp Suite Professional

## 📖 Overview

This repository documents a comprehensive **Web Application Security Assessment** performed against the intentionally vulnerable **OWASP Juice Shop** application using **Burp Suite Professional**.

The assessment demonstrates a structured manual web application penetration testing methodology by identifying, analyzing, and validating security controls across multiple application components. Testing focused on authentication, authorization, endpoint discovery, parameter manipulation, business logic validation, input validation, error handling, and functional verification.

All testing was conducted manually within a controlled laboratory environment using industry-standard web application penetration testing techniques.

---

# 🎯 Assessment Objectives

The primary objectives of this assessment were to:

- Deploy OWASP Juice Shop in a local Docker environment
- Configure Burp Suite Professional as an intercepting proxy
- Capture and analyze HTTP/HTTPS traffic
- Enumerate application endpoints
- Assess authentication and authorization mechanisms
- Perform manual parameter manipulation
- Evaluate server-side input validation
- Assess business logic implementation
- Verify application functionality
- Identify security weaknesses
- Document findings with supporting evidence

---

# 🖥️ Lab Environment

| Component | Details |
|------------|---------|
| Host Operating System | Windows 11 |
| Web Proxy | Burp Suite Professional |
| Browser | Burp Suite Built-in Browser |
| Target Application | OWASP Juice Shop |
| Deployment Method | Docker Container |
| Docker Image | bkimminich/juice-shop |
| Local URL | http://localhost:3000 |

---

# 🛠 Tools Used

- Burp Suite Professional
- Docker Desktop
- OWASP Juice Shop
- Burp Suite Built-in Browser

---

# 📦 Lab Setup

## Pull Docker Image

```bash
docker pull bkimminich/juice-shop
```

## Start Juice Shop

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

## Verify Running Container

```bash
docker ps
```

## Access Application

```
http://localhost:3000
```

---

# ⚙️ Burp Suite Configuration

The testing environment was configured using Burp Suite Professional.

Configuration included:

- Proxy Listener
- Burp CA Certificate Installation
- Burp Suite Browser
- HTTP History
- Target Scope
- Repeater
- Intruder
- Decoder
- Comparer

---

# 🔍 Assessment Methodology

The assessment followed a structured manual web application penetration testing methodology.

## Phase 1 – Environment Preparation

- Docker deployment
- Burp Suite configuration
- Certificate installation
- Browser configuration
- Proxy verification

---

## Phase 2 – Application Reconnaissance

- Browse application
- Build Target Site Map
- Discover API endpoints
- Analyze application structure
- Define assessment scope

---

## Phase 3 – Authentication Assessment

- User registration
- Login workflow analysis
- JWT acquisition
- Session validation
- Authentication verification

---

## Phase 4 – Authorization Assessment

- JWT validation
- Cookie manipulation
- Authorization header testing
- Administrative endpoint testing
- IDOR verification
- Access control validation

---

## Phase 5 – Directory Enumeration

- Endpoint discovery
- API enumeration
- Resource identification
- Site Map analysis

---

## Phase 6 – Parameter Manipulation

Manual parameter manipulation was performed across multiple endpoints including:

- Basket
- Address
- Card
- Checkout
- Feedback
- Product Search

Testing included:

- Missing parameters
- Null values
- Invalid data types
- Numeric boundary testing
- Authentication parameter manipulation
- SQL Injection payloads
- Cross-Site Scripting payloads
- Business logic validation

---

## Phase 7 – Functional Verification

Functional testing verified the application's primary workflows, including:

- Product Search
- Basket Management
- Address Management
- Payment Card Management
- Checkout
- Order History
- Order Tracking
- Feedback Submission
- Complaint Submission

---

## Phase 8 – Security Validation

The assessment concluded by validating:

- Server-side input validation
- Authentication mechanisms
- Authorization controls
- Business logic
- Error handling
- Information disclosure
- Security observations

---

# 🔎 Burp Suite Components Used

| Component | Purpose |
|------------|---------|
| Proxy | Intercept HTTP/HTTPS traffic |
| HTTP History | Review captured requests |
| Target Site Map | Discover application endpoints |
| Repeater | Manual request modification |
| Intruder | Automated payload testing |
| Decoder | Encode/Decode data |
| Comparer | Compare requests and responses |

---

# 📂 Assessment Modules

The repository contains detailed reports for the following assessment areas:

- Login Request Testing
- Authorization Testing
- Directory Enumeration
- Parameter Manipulation
- Search Parameter Testing
- Basket Parameter Testing
- Address Parameter Testing
- Card Parameter Testing
- Checkout Parameter Testing
- Feedback Parameter Testing
- Functional Testing

Each module contains:

- Objective
- Methodology
- Burp Suite workflow
- Test cases
- Security observations
- Supporting screenshots
- Conclusion

---

# 📊 Security Assessment Coverage

| Assessment Area | Status |
|-----------------|--------|
| Authentication Testing | ✅ Completed |
| Authorization Testing | ✅ Completed |
| JWT Validation | ✅ Completed |
| Cookie Manipulation | ✅ Completed |
| IDOR Verification | ✅ Completed |
| Directory Enumeration | ✅ Completed |
| Parameter Manipulation | ✅ Completed |
| Search Parameter Testing | ✅ Completed |
| Basket Testing | ✅ Completed |
| Address Testing | ✅ Completed |
| Card Testing | ✅ Completed |
| Checkout Testing | ✅ Completed |
| Feedback Testing | ✅ Completed |
| SQL Injection Testing | ✅ Completed |
| Cross-Site Scripting (XSS) Testing | ✅ Completed |
| Input Validation Assessment | ✅ Completed |
| Business Logic Testing | ✅ Completed |
| Error Handling Assessment | ✅ Completed |
| Information Disclosure Assessment | ✅ Completed |
| Functional Testing | ✅ Completed |

---

# 📁 Repository Structure

```
OWASP-Juice-Shop-Security-Assessment/
│
├── Screenshots/
├── Security_Testing/
│   ├── Login_Request_Testing/
│   ├── Authorization_Testing/
│   ├── Directory_Enumeration/
│   ├── Parameter_Manipulation/
│   ├── Search_Parameter_Testing/
│   ├── Basket_Parameter_Testing/
│   ├── Address_Parameter_Testing/
│   ├── Card_Parameter_Testing/
│   ├── Checkout_Parameter_Testing/
│   └── Feedback_Parameter_Testing/
│
├── Functional_Testing/
├── REPORT.md
└── README.md
```

---

# 📄 Assessment Report

The complete assessment, including testing methodology, observations, screenshots, findings, and conclusions, is documented within the individual assessment modules and the accompanying **[REPORT.md](REPORT.md)**.

---

# 📚 Skills Demonstrated

This project demonstrates practical experience in:

- Web Application Penetration Testing
- Burp Suite Professional
- Docker-based Lab Deployment
- HTTP/HTTPS Traffic Analysis
- Authentication Testing
- Authorization Testing
- JWT Analysis
- Directory Enumeration
- Parameter Manipulation
- SQL Injection Testing
- Cross-Site Scripting (XSS) Testing
- Business Logic Testing
- IDOR Verification
- Input Validation Assessment
- Error Handling Analysis
- Functional Testing
- Technical Documentation
- Security Reporting

---

# ⚠️ Disclaimer

This assessment was conducted exclusively within a controlled laboratory environment using **OWASP Juice Shop**, an intentionally vulnerable application developed for security awareness, education, and penetration testing practice.

No testing was performed against unauthorized systems or production environments.

---

# 👨‍💻 Author

**Gautam S B**

Aspiring Cybersecurity Engineer | VAPT Enthusiast

**GitHub:** https://github.com/GautamSB

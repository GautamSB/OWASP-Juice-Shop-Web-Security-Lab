# OWASP Juice Shop Web Application Security Assessment using Burp Suite

## 📖 Overview

This repository documents a comprehensive Web Application Security Assessment performed against the intentionally vulnerable **OWASP Juice Shop** application using **Burp Suite Professional**.

The assessment demonstrates a structured penetration testing methodology by identifying, analyzing, and validating common web application vulnerabilities through manual testing techniques. The objective was to understand application behavior, manipulate HTTP requests, evaluate server responses, and assess the application's security posture using industry-standard web security testing practices.

---

# 🎯 Assessment Objectives

The objectives of this assessment were to:

- Configure Burp Suite as an intercepting proxy
- Capture and analyze HTTP/HTTPS requests
- Map the application's attack surface
- Perform authentication testing
- Analyze request and response behavior
- Manipulate application parameters
- Validate server-side input handling
- Perform manual vulnerability verification
- Use Burp Suite tools for security assessment
- Document findings with supporting evidence

---

# 🖥️ Lab Environment

| Component | Details |
|-----------|---------|
| Operating System | Windows 11 |
| Web Browser | Google Chrome |
| Proxy Extension | FoxyProxy |
| Web Proxy | Burp Suite Professional |
| Target Application | OWASP Juice Shop |
| Deployment | Docker Container |
| Docker Image | bkimminich/juice-shop |
| Local URL | http://localhost:3000 |

---

# 🛠 Tools Used

- Burp Suite Professional
- Docker Desktop
- OWASP Juice Shop
- Google Chrome
- FoxyProxy

---

# 📦 Lab Setup

## Pull the OWASP Juice Shop Docker Image

```bash
docker pull bkimminich/juice-shop
```

## Run the Docker Container

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

## Access the Application

```
http://localhost:3000
```

---

# ⚙️ Burp Suite Configuration

The testing environment was configured using Burp Suite Professional as the intercepting proxy.

Configuration included:

- Proxy Listener (127.0.0.1:8080)
- Burp CA Certificate Installation
- FoxyProxy Browser Configuration
- Target Scope Definition
- HTTP History Monitoring
- Repeater
- Intruder
- Decoder
- Comparer

---

# 🔍 Assessment Methodology

The assessment followed a structured penetration testing approach consisting of the following phases:

### Phase 1 – Environment Preparation

- Configure Docker
- Deploy OWASP Juice Shop
- Configure Burp Suite
- Configure FoxyProxy
- Install Burp Certificate
- Verify proxy interception

### Phase 2 – Reconnaissance

- Browse the application
- Build target site map
- Analyze endpoints
- Review HTTP history
- Identify attack surface

### Phase 3 – Authentication Testing

- Capture login requests
- Analyze authentication workflow
- Modify authentication parameters
- Observe server responses
- Test login functionality

### Phase 4 – Input Validation Testing

- Manipulate request parameters
- Test client-side validation
- Verify server-side validation
- Analyze error handling

### Phase 5 – Parameter Manipulation

- Modify HTTP requests
- Replay requests using Repeater
- Compare server responses
- Validate parameter handling

### Phase 6 – Intruder Testing

- Configure attack positions
- Perform payload testing
- Analyze response variations
- Identify abnormal behavior

### Phase 7 – Vulnerability Validation

- Verify identified issues
- Capture supporting evidence
- Analyze impact
- Document observations

---

# 🔎 Burp Suite Components Used

| Component | Purpose |
|-----------|---------|
| Proxy | Capture and intercept HTTP requests |
| HTTP History | Review application traffic |
| Target | Map application endpoints |
| Repeater | Modify and replay requests |
| Intruder | Automated payload testing |
| Decoder | Encode and decode data |
| Comparer | Compare responses |

---

# 📊 Assessment Deliverables

The assessment includes:

- Burp Suite configuration
- HTTP request interception
- Application mapping
- Request modification
- Response analysis
- Parameter manipulation
- Authentication testing
- Intruder testing
- Screenshots
- Detailed assessment report

---

# 📄 Detailed Report

The complete assessment methodology, request analysis, observations, findings, screenshots, and recommendations are available in:

**REPORT.md**

---

# 📸 Screenshots

All screenshots demonstrating the assessment process are available in the **Screenshots/** directory.

The screenshots include:

- Docker deployment
- Burp Suite configuration
- FoxyProxy setup
- Proxy interception
- HTTP History
- Site Map
- Authentication requests
- Repeater testing
- Intruder testing
- Response comparison
- Vulnerability validation

---

# 📚 Learning Outcomes

After completing this assessment, I gained practical experience in:

- Web Application Penetration Testing
- Burp Suite Professional
- HTTP Request Analysis
- Web Proxy Configuration
- Request Manipulation
- Authentication Testing
- Parameter Tampering
- Manual Vulnerability Verification
- Response Analysis
- Professional Security Reporting

---

## 👨‍💻 Author

**Gautam S B**

Aspiring Cybersecurity Engineer | VAPT Enthusiast

GitHub: https://github.com/GautamSB

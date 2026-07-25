# OWASP Juice Shop Web Application Security Assessment using Burp Suite

## 📖 Overview

This repository documents a comprehensive Web Application Security Assessment performed against the intentionally vulnerable **OWASP Juice Shop** application using **Burp Suite Professional**.

The assessment demonstrates a structured penetration testing methodology by identifying, analyzing, and validating common web application vulnerabilities through manual testing techniques. The objective was to understand application behavior, intercept and manipulate HTTP requests, evaluate server responses, and assess the application's security posture using industry-standard web application penetration testing practices.

---

# 🎯 Assessment Objectives

The objectives of this assessment were to:

- Configure Burp Suite as an intercepting proxy
- Deploy OWASP Juice Shop using Docker
- Capture and analyze HTTP/HTTPS traffic
- Map the application's attack surface
- Perform authentication testing
- Analyze request and response behavior
- Manipulate HTTP parameters
- Validate server-side input handling
- Verify common web application vulnerabilities
- Document findings with supporting evidence

---

# 🖥️ Lab Environment

| Component | Details |
|-----------|---------|
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

## Pull the OWASP Juice Shop Docker Image

```bash
docker pull bkimminich/juice-shop
```

## Run the Docker Container

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

## Verify Running Container

```bash
docker ps
```

## Access the Application

```
http://localhost:3000
```

---

# ⚙️ Burp Suite Configuration

The assessment environment was configured using Burp Suite Professional as the primary interception platform.

Configuration included:

- Proxy Listener (127.0.0.1:8080)
- Burp CA Certificate Installation
- Burp Suite Built-in Browser
- Target Scope Configuration
- HTTP History Monitoring
- Repeater
- Intruder
- Decoder
- Comparer

---

# 🔍 Assessment Methodology

The assessment followed a structured web application penetration testing methodology.

## Phase 1 — Environment Preparation

- Deploy OWASP Juice Shop using Docker
- Configure Burp Suite Professional
- Install Burp CA Certificate
- Launch Burp Built-in Browser
- Verify proxy interception
- Confirm HTTP traffic capture

---

## Phase 2 — Reconnaissance

- Browse the application
- Build the application Site Map
- Identify exposed endpoints
- Analyze HTTP requests
- Review application functionality
- Define testing scope

---

## Phase 3 — Authentication Testing

- Capture login requests
- Analyze authentication workflow
- Modify authentication parameters
- Observe server responses
- Evaluate authentication controls

---

## Phase 4 — Input Validation Testing

- Test client-side validation
- Validate server-side input filtering
- Analyze error messages
- Manipulate request parameters
- Observe response behavior

---

## Phase 5 — Parameter Manipulation

- Modify HTTP requests
- Replay requests using Repeater
- Compare application responses
- Validate parameter handling

---

## Phase 6 — Intruder Testing

- Configure payload positions
- Execute automated payload attacks
- Analyze response differences
- Identify abnormal application behavior

---

## Phase 7 — Vulnerability Validation

- Verify identified vulnerabilities
- Capture evidence
- Assess security impact
- Document findings
- Recommend remediation

---

# 🔎 Burp Suite Components Used

| Component | Purpose |
|-----------|---------|
| Proxy | Intercept HTTP/HTTPS traffic |
| HTTP History | Analyze application requests |
| Target | Build application Site Map |
| Repeater | Modify and replay HTTP requests |
| Intruder | Automated payload testing |
| Decoder | Encode and decode data |
| Comparer | Compare requests and responses |

---

# 📊 Assessment Deliverables

This assessment includes:

- Docker deployment
- Burp Suite configuration
- HTTP request interception
- Site Map generation
- Authentication testing
- Request manipulation
- Response analysis
- Parameter tampering
- Intruder testing
- Evidence collection
- Professional assessment report

---

# 📄 Detailed Report

The complete penetration testing process, observations, request analysis, screenshots, findings, and remediation recommendations are documented in:

**REPORT.md**

---

# 📸 Screenshots

The **Screenshots/** directory contains evidence collected throughout the assessment.

Included screenshots:

- Docker image deployment
- Running Docker container
- Burp Suite proxy configuration
- Burp Built-in Browser
- HTTP request interception
- HTTP History
- Target Site Map
- Authentication testing
- Repeater request modification
- Intruder attack configuration
- Response comparison
- Vulnerability validation
- Assessment evidence

---

# 📚 Learning Outcomes

This assessment provided hands-on experience in:

- Web Application Penetration Testing
- Burp Suite Professional
- Docker-based Lab Deployment
- HTTP/HTTPS Traffic Analysis
- Request Interception
- Manual Request Manipulation
- Authentication Testing
- Input Validation Testing
- Parameter Tampering
- Intruder-based Testing
- Manual Vulnerability Verification
- Professional Security Reporting

---

# ⚠️ Disclaimer

This assessment was conducted exclusively within a controlled laboratory environment using **OWASP Juice Shop**, an intentionally vulnerable application designed for security education and penetration testing practice. No testing was performed against unauthorized systems.

---

## 👨‍💻 Author

**Gautam S B**

Aspiring Cybersecurity Engineer | VAPT Enthusiast

GitHub: https://github.com/GautamSB

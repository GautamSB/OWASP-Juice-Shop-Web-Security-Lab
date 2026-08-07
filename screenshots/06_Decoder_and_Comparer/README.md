# Burp Suite Decoder & Comparer Demonstration

## Objective

Demonstrate the use of **Burp Suite Decoder** and **Burp Suite Comparer** during the OWASP Juice Shop Web Application Security Assessment. These tools were used to analyze encoded application data, inspect authentication tokens, compare HTTP requests and responses, and identify differences between legitimate and manipulated application behavior.

---

# Burp Suite Tools Used

- **Decoder** – Decoded and analyzed JWT tokens and encoded HTTP data.
- **Comparer** – Compared HTTP requests, HTTP responses, and authentication tokens to identify differences during security testing.

---

# Decoder Demonstration

## 1. JWT Token Analysis

JWT tokens generated after successful user authentication were decoded using Burp Suite Decoder.

The decoded token revealed information including:

- JWT Header
- Payload
- User ID
- Email Address
- Issued Time (`iat`)
- Expiration Time (`exp`)

### Observation

Burp Decoder successfully decoded the Base64URL-encoded JWT, allowing inspection of the authentication claims without modifying the original token.

---

## 2. URL Encoding and Decoding

URL-encoded data was processed using Burp Decoder to verify how special characters are encoded and decoded during HTTP communication.

### Observation

Burp Decoder accurately converted encoded values into their readable form and vice versa, assisting in payload preparation during testing.

---

## 3. Base64 Encoding and Decoding

Base64-encoded data was analyzed to understand how application data is represented within HTTP requests and responses.

### Observation

Decoder successfully converted Base64 data into plaintext and encoded plaintext into Base64 format for testing purposes.

---

# Comparer Demonstration

## 1. JWT Token Comparison

Authentication tokens generated for two different user accounts were compared using Burp Suite Comparer.

Accounts used:

- **test@gmail.com**
- **example@gmail.com**

### Observation

Comparer highlighted differences within the JWT payload, including:

- User ID
- Email Address
- Issued Time (`iat`)
- Expiration Time (`exp`)
- Digital Signature

This confirmed that each authenticated user received a unique JWT.

---

## 2. HTTP Request and Response Comparison

HTTP requests and responses captured during manual testing were compared to identify differences between legitimate and modified requests.

Comparison included:

- Normal application requests
- Manipulated requests
- Corresponding server responses

### Observation

Comparer visually highlighted modified parameters, response differences, HTTP status changes, and returned data, simplifying the verification of security testing results.

---

# Summary of Activities

| Activity | Result |
|----------|--------|
| JWT Token Decoding | ✅ Successfully decoded |
| URL Encoding / Decoding | ✅ Successful |
| Base64 Encoding / Decoding | ✅ Successful |
| JWT Comparison | ✅ User differences identified |
| HTTP Request Comparison | ✅ Request differences identified |
| HTTP Response Comparison | ✅ Response differences identified |

---

# Skills Demonstrated

- Burp Suite Decoder
- Burp Suite Comparer
- JWT Analysis
- Base64 Encoding and Decoding
- URL Encoding and Decoding
- HTTP Request Analysis
- HTTP Response Analysis
- Authentication Token Inspection
- Manual Web Application Security Testing

---

# Evidence Captured

- JWT Token Decoding
- URL Encoding and Decoding
- Base64 Encoding and Decoding
- JWT Token Comparison
- HTTP Request Comparison
- HTTP Response Comparison

---

# Conclusion

Burp Suite Decoder and Burp Suite Comparer were used to support manual web application security testing by decoding authentication tokens, analyzing encoded application data, and comparing HTTP requests and responses captured during the assessment.

Decoder simplified the inspection of encoded data such as JWT tokens and Base64 values, while Comparer enabled efficient identification of differences between legitimate and manipulated requests and responses. Together, these tools enhanced the analysis of authentication mechanisms, request manipulation, and application behavior throughout the OWASP Juice Shop security assessment.

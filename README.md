# 🔐 Task 13 – Secure API Testing & Authorization Validation

**Internship:** Cyber Security Internship
**Platform:** Kali Linux
**Tools Used:** Postman (Lightweight API Client), cURL
**Test APIs:** jsonplaceholder.typicode.com, httpbin.org

---

# 📌 Objective

To perform secure API testing and validate:

* Authentication mechanisms
* Authorization controls (IDOR testing)
* Input validation handling
* Rate limiting enforcement

Findings are mapped to **OWASP API Security Top 10** risks.

---

# 🖥 Environment Setup

## Postman Installation

```bash
sudo snap install postman
```

Verified installation and resolved snap warning.

## DNS Fix (Connection Reset Issue)

Edited `/etc/resolv.conf`:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

This resolved connection reset errors.

---

# 🔎 API Security Testing Steps

---

# 1️⃣ GET Request Testing

**Endpoint:**

```
https://jsonplaceholder.typicode.com/posts
```

**Result:**

* Status: 200 OK
* JSON data retrieved successfully

**Screenshot:**
`01_get_request_200.png`

---

# 2️⃣ POST Request Testing

**Endpoint:**

```
https://jsonplaceholder.typicode.com/posts
```

**Request Body:**

```json
{
  "title": "test",
  "body": "api security testing",
  "userId": 1
}
```

**Result:**

* Status: 201 Created

**Screenshot:**
`02_post_request_201.png`

---

# 3️⃣ Authentication Testing

**Endpoint:**

```
https://httpbin.org/basic-auth/user/passwd
```

### Valid Credentials

* Username: user
* Password: passwd
* Result: 200 OK

**Screenshot:**
`03_auth_valid_200.png`

---

### Invalid Credentials

* Password changed
* Result: 401 Unauthorized

**Screenshot:**
`04_auth_invalid_401.png`

---

# 4️⃣ Authorization Testing (IDOR)

Tested:

```
/posts/1
/posts/2
```

Observation:

* Data accessible by changing ID
* No authorization validation

**Screenshots:**
`05_idor_post_1.png`
`06_idor_post_2.png`

Mapped to:
**OWASP API1 – Broken Object Level Authorization**

---

# 5️⃣ Input Validation Testing

Sent malicious payload:

```json
{
  "title": "<script>alert('XSS')</script>",
  "body": "' OR 1=1 --",
  "userId": 1
}
```

Result:

* Status: 200 OK
* Input accepted without sanitization

**Screenshot:**
`07_injection_test.png`

Mapped to:
**OWASP API8 – Injection**

---

# 6️⃣ Rate Limiting Testing

Command executed:

```bash
for i in {1..40}; do curl -s -o /dev/null -w "%{http_code}\n" https://jsonplaceholder.typicode.com/posts; done
```

Observation:

* All responses returned 200
* No 429 Too Many Requests
* No blocking mechanism

**Screenshot:**
`08_rate_limit_test.png`

Mapped to:
**OWASP API4 – Lack of Resources & Rate Limiting**

---

# 🛡 Vulnerability Summary

| Test Performed   | Result            | OWASP Category |
| ---------------- | ----------------- | -------------- |
| Authentication   | Properly enforced | API2           |
| ID Manipulation  | Data accessible   | API1           |
| Input Validation | No sanitization   | API8           |
| Rate Limiting    | Not implemented   | API4           |



# 🎯 Conclusion

This task demonstrated practical API security testing including:

* Authentication validation
* Authorization flaw detection (IDOR)
* Injection testing
* Rate limiting assessment
* HTTP response code analysis

The API was successfully evaluated against OWASP API Security Top 10 risks.

---

# 🎤 Interview Preparation

**What is API authentication?**
Verification of user identity before granting API access.

**What is broken authorization?**
When users can access resources they are not permitted to access.

**Why is rate limiting important?**
Prevents brute-force and denial-of-service attacks.

**Difference between GET and POST?**
GET retrieves data; POST submits or creates data.

---

# ✅ Task 13 Completed Successfully

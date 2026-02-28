# Task 03 – API Security Risk Analysis

## Objective
To analyze a public test API endpoint and identify potential security risks and misconfigurations.

---

## 1. API Target
API: JSONPlaceholder (Public Test API)  
Base URL: https://jsonplaceholder.typicode.com

---

## 2. Testing Methodology
Tools:
- curl (Debian terminal)

Checks performed:
- Endpoint enumeration (common routes)
- Authentication requirement (presence/absence)
- Rate limiting behavior (basic observation)
- Security headers (basic)
- Error handling behavior
- Data exposure risks (PII/over-sharing)

---

## 3. API Tests & Observations

### 3.1 Test 1 — Public Data Access (No Auth)
Command:
curl -i https://jsonplaceholder.typicode.com/users

Observation:
(To be filled)

Risk:
(To be filled)

### 3.2 Test 2 — Insecure Direct Object Reference (IDOR-like check)
Command:
curl -i https://jsonplaceholder.typicode.com/users/1

Observation:
(To be filled)

Risk:
(To be filled)

### 3.3 Test 3 — Error Handling / Information Disclosure
Command:
curl -i https://jsonplaceholder.typicode.com/users/9999

Observation:
(To be filled)

Risk:
(To be filled)

### 3.4 Test 4 — Security Headers
Command:
curl -I https://jsonplaceholder.typicode.com/users

Observation:
(To be filled)

Risk:
(To be filled)

---

## 4. Risk Assessment
(To be filled)

---

## 5. Security Recommendations
(To be filled)

---

## 6. Evidence
(To be filled with screenshots)

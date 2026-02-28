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
curl -i https://jsonplaceholder.typicode.com/users | head -n 25

Observation:
- HTTP/2 200 returned successfully.
- The endpoint returns user records without any authentication challenge.
- Rate limiting headers are present (x-ratelimit-limit, x-ratelimit-remaining).
- The response includes "x-powered-by: Express" indicating technology disclosure.

Risk:
- If this were a real production API, public access to user records could result in data exposure.
- Technology disclosure (x-powered-by) helps attackers tailor exploits to the stack.

### 3.2 Test 2 — Insecure Direct Object Reference (IDOR-like check)
Command:
curl -i https://jsonplaceholder.typicode.com/users/1 | head -n 25

Observation:
- HTTP/2 200 returned successfully.
- Direct object access by ID (users/1) returns the user object without authentication.

Risk:
- In real systems, predictable IDs combined with missing authorization enables IDOR attacks (enumeration of other users’ data).

### 3.3 Test 3 — Error Handling / Information Disclosure
Command:
curl -i https://jsonplaceholder.typicode.com/users/9999 | head -n 25

Observation:
- HTTP/2 404 returned.
- Response body is "{}" (minimal error detail).

Risk:
- Low: The API does not leak stack traces or detailed internal errors. This is good secure behavior.

### 3.4 Test 4 — Security Headers
Command:
curl -I https://jsonplaceholder.typicode.com/users

Observation:
- Security header present: x-content-type-options: nosniff
- Rate limiting headers present (x-ratelimit-*)
- Technology disclosure present: x-powered-by: Express
- access-control-allow-credentials: true is enabled (could be risky if combined with permissive Origin policy).
- Common security headers were not observed (e.g., strict-transport-security, content-security-policy, x-frame-options).

Risk:
- Missing security headers may increase exposure depending on how the API is used.
- CORS with credentials can be dangerous if misconfigured (can enable cross-site data access with user credentials).

---

## 4. Risk Assessment

Overall Risk Level: Medium

Reasoning:
- Unauthenticated access to user resources and ID-based endpoints is acceptable for a public test API, but would be a high-risk issue in production.
- Some protections exist (rate limiting headers, minimal error detail).
- Security header coverage is incomplete and technology disclosure is present.

---
## 5. Security Recommendations

1. Require authentication (API keys/OAuth2/JWT) for endpoints returning user or sensitive data.
2. Enforce object-level authorization checks to prevent IDOR (/resource/{id} must verify permissions).
3. Reduce information disclosure:
   - Remove or minimize “x-powered-by” header exposure.
4. Improve security headers where relevant:
   - Add HSTS (strict-transport-security) where applicable.
   - Consider a security header baseline for responses.
5. Review CORS configuration:
   - If credentials are allowed, strictly restrict allowed origins (never use wildcard with credentials).
6. Implement server-side rate limiting and monitoring to detect abuse.

---

---

---

## 6. Testing Environment

Operating System: Debian Linux  
Tool Used: curl (command-line HTTP client)  
API Target: JSONPlaceholder (Public Test API)  
Testing Scope: Passive observation and endpoint inspection only.  
No authentication bypass, exploitation, or brute-force testing was performed.

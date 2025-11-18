# Lesson 02: All Security Protocols Prompts

**Complete collection of 9 prompts for authentication, encryption, and API security**

---

## 1. OAuth 2.0 Server with PKCE

```text
Generate a complete OAuth 2.0 authorization server implementation for Python/Django with:
1. PKCE support (code_verifier and code_challenge using SHA-256)
2. Authorization code flow with proper state parameter validation
3. Access token and refresh token issuance using JWT with RS256 signing
4. Token expiration (access: 15 minutes, refresh: 7 days)
5. Token rotation on refresh
6. Comprehensive error handling with OAuth 2.0 error codes

Include detailed inline comments explaining each security control.
Use the authlib library for OAuth primitives.
```

**Use for**: Implementing OAuth 2.0 authentication
**Customization**: Replace "Python/Django" with your framework, "authlib" with your library

---

## 2. JWT Validation Middleware

```text
Create Django middleware that validates JWT access tokens for all API endpoints.
The middleware should:
1. Extract Bearer token from Authorization header
2. Validate JWT signature using RS256 with public key
3. Check token expiration with 5-minute clock skew tolerance
4. Verify all required claims (iss, sub, aud, exp, iat, jti)
5. Prevent token reuse by checking jti (JWT ID) against revocation list
6. Set request.user based on validated token
7. Return proper 401/403 responses with WWW-Authenticate header

Add comprehensive error handling and logging.
```

**Use for**: Protecting API endpoints with JWT validation
**Customization**: Replace "Django" with Flask, FastAPI, Express.js, Spring, etc.

---

## 3. OAuth Security Tests

```text
Generate pytest security tests for the OAuth 2.0 server that verify:
1. Authorization code cannot be reused
2. Invalid PKCE verifier is rejected
3. Expired tokens are rejected
4. Token signature tampering is detected
5. Missing claims cause validation failure
6. Token refresh requires valid refresh token
7. Scope enforcement works correctly

Use realistic attack scenarios and edge cases.
```

**Use for**: Testing OAuth implementation security
**Customization**: Replace "pytest" with JUnit, Jest, PHPUnit, etc.

---

## 4. AES-256-GCM Encryption with Azure Key Vault

```text
Create a Python encryption service class that:
1. Uses AES-256-GCM for encryption (provides authenticity + confidentiality)
2. Generates unique 96-bit IV for each encryption operation using secrets module
3. Integrates with Azure Key Vault for key management using DefaultAzureCredential
4. Implements key rotation with dual encryption for transition period
5. Handles initialization vector and authentication tag properly
6. Includes decrypt method with authentication tag verification
7. Provides key caching with 1-hour TTL to reduce Key Vault calls
8. Includes comprehensive error handling for Key Vault failures

Add type hints and docstrings following Google style.
Save as utils/encryption_service.py
```

**Use for**: Field-level encryption with cloud KMS
**Customization**: Replace "Azure Key Vault" with AWS KMS, GCP Secret Manager, HashiCorp Vault

---

## 5. Password Hashing with bcrypt

```text
Generate a password hashing utility for PyGoat that:
1. Uses bcrypt with work factor of 12 (appropriate for 2025)
2. Implements password verification with timing attack protection
3. Includes password strength validation (NIST 800-63B requirements)
4. Provides password rotation checks
5. Logs failed authentication attempts for security monitoring

Include usage examples and security best practices in docstrings.
```

**Use for**: Secure password storage
**Customization**: Can adapt for Argon2, PBKDF2, or scrypt

---

## 6. Rate Limiting Middleware

```text
Create Django middleware for API rate limiting that implements:
1. Sliding window algorithm (not fixed window - prevents burst circumvention)
2. Redis-backed distributed rate limiting for horizontal scaling
3. Different limits per endpoint (e.g., /login: 5/min, /api: 100/min)
4. Per-user and per-IP rate limiting
5. Exponential backoff for repeated violations
6. Custom headers (X-RateLimit-Limit, X-RateLimit-Remaining, Retry-After)
7. Integration with Django's cache framework
8. Graceful degradation if Redis is unavailable (fail open but log)

Use atomic Redis operations to prevent race conditions.
```

**Use for**: API rate limiting and DoS protection
**Customization**: Replace "Django" with Express, Flask, Spring; "Redis" with Memcached, in-memory

---

## 7. Request Validation Middleware

```text
Create middleware that validates all API requests for:
1. Content-Type header validation (reject non-JSON for API endpoints)
2. Request body size limits (max 1MB to prevent DoS)
3. JSON schema validation against OpenAPI specs
4. Header injection detection (newlines in headers)
5. SQL injection patterns in all input fields
6. XSS patterns in text inputs
7. Path traversal attempts in file parameters
8. IDOR prevention by validating resource ownership

Return proper 400 Bad Request with detailed validation errors.
Integrate with Django REST framework validators.
```

**Use for**: Input validation and attack prevention
**Customization**: Replace framework-specific validators

---

## 8. CORS Security Configuration

```text
Configure Django CORS middleware with security best practices:
1. Whitelist specific origins (no wildcards in production)
2. Limit allowed methods to only what's needed
3. Set Access-Control-Max-Age appropriately
4. Handle preflight requests correctly
5. Implement origin validation with subdomain support
6. Add security headers (HSTS, X-Content-Type-Options, etc.)

Include configuration for both development and production environments.
```

**Use for**: Secure cross-origin API access
**Customization**: Adapt for any web framework

---

## 9. Custom CodeQL Queries for Python

```text
Generate a CodeQL query for Python that detects:
1. Django views missing authentication decorators
2. Hard-coded cryptographic keys in source code
3. SQL queries using string concatenation instead of parameterized queries
4. Sensitive data (SSN, credit card patterns) in logging statements

Provide the query in CodeQL syntax with explanatory comments.
Save as .github/codeql/custom-queries/python-security.ql
```

**Use for**: Organization-specific SAST rules
**Customization**: Replace "Python" with Java, JavaScript, Go; adapt detection patterns

---

## Quick Reference: When to Use Each Prompt

| Security Need | Prompt # | Prompt Name |
|--------------|----------|-------------|
| User Authentication | 1-3 | OAuth + JWT + Tests |
| Data Encryption | 4 | AES-GCM with Key Vault |
| Password Storage | 5 | bcrypt Hashing |
| DoS Protection | 6 | Rate Limiting |
| Input Validation | 7 | Request Validation |
| Cross-Origin Access | 8 | CORS Configuration |
| Custom Security Rules | 9 | CodeQL Queries |

---

## Compliance Mappings

- **OAuth 2.0 + PKCE**: RFC 6749, RFC 7636, NIST 800-63B
- **AES-256-GCM**: NIST 800-53 SC-28, PCI-DSS 3.4
- **Password Hashing**: NIST 800-63B, OWASP ASVS
- **Rate Limiting**: OWASP API Security Top 10 API4:2023
- **Input Validation**: OWASP Top 10 A03:2021, CWE-20

---

[← Back to Lesson 02 Index](./README.md) | [Next: Lesson 03 →](../Lesson-03-Automated-Testing/)

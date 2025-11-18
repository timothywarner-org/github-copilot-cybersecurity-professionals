# Lesson 02: Security Protocols Prompts

**9 prompts for implementing authentication, encryption, and API security**

## Overview

These prompts help you use GitHub Copilot to:
- Implement OAuth 2.0 with PKCE
- Create JWT validation middleware
- Build encryption services with Key Vault integration
- Implement password hashing with bcrypt
- Create API gateway security (rate limiting, validation, CORS)
- Generate custom CodeQL queries

**Target Application**: PyGoat (deliberately vulnerable Python/Django app)
**Skill Level**: Advanced
**Time to Complete**: 15-20 minutes

---

## Prompts in This Lesson

### OAuth 2.0 & Authentication (3 prompts)
1. [OAuth 2.0 Server with PKCE Implementation](./01-OAuth-PKCE-Implementation.md)
2. [JWT Validation Middleware](./02-JWT-Validation-Middleware.md)
3. [OAuth Security Tests](./03-OAuth-Security-Tests.md)

### Encryption & Key Management (3 prompts)
4. [AES-256-GCM Encryption with Azure Key Vault](./04-Encryption-Service.md)
5. [Password Hashing with bcrypt](./05-Password-Hashing.md)
6. [Secret Scanning Validation](./06-Secret-Scanning.md)

### API Gateway Security (3 prompts)
7. [Rate Limiting Middleware](./07-Rate-Limiting.md)
8. [Request Validation Middleware](./08-Request-Validation.md)
9. [CORS Security Configuration](./09-CORS-Security.md)

### GHAS Integration
10. [Custom CodeQL Queries for Python](./10-CodeQL-Custom-Queries.md)

---

## Quick Start

### Prerequisites
```bash
git clone https://github.com/adeyosemanputra/pygoat.git
cd pygoat
pip install -r requirements.txt
pip install pyjwt cryptography python-jose azure-identity azure-keyvault-secrets bcrypt
```

### Typical Workflow

```
1. Authentication → Prompts 1-3 (OAuth + JWT + Tests)
2. Encryption     → Prompts 4-6 (AES + Passwords + Secrets)
3. API Security   → Prompts 7-9 (Rate Limit + Validation + CORS)
4. Code Analysis  → Prompt 10 (Custom CodeQL)
```

---

## Key Concepts

### Security Requirements in Prompts
Always specify:
- **Cipher modes**: "AES-256-GCM" not just "AES"
- **Token expiration**: "15 min access, 7 day refresh"
- **Key lengths**: "RSA-2048" or "RS256"
- **Standards**: "RFC 6749", "PKCE RFC 7636", "NIST 800-53"

### Production-Grade Code
Request:
- Error handling and logging
- Type hints and docstrings
- Validation and edge cases
- Security best practices
- Compliance mappings

---

## Customization Guide

### Adapting for Different Frameworks

**Django → Flask**:
```text
Create Flask middleware that validates JWT access tokens...
```

**Django → FastAPI**:
```text
Create FastAPI dependency for JWT validation using OAuth2PasswordBearer...
```

### Adapting for Different Cloud Providers

**Azure Key Vault → AWS KMS**:
```text
Create encryption service using AWS KMS for key management with boto3.
Use AES-256-GCM with unique IV per encryption...
```

**Azure → GCP Secret Manager**:
```text
Integrate with GCP Secret Manager using google-cloud-secret-manager.
Implement key caching with 1-hour TTL...
```

---

[← Back to Prompts Home](../README.md) | [Next: Lesson 03 Automated Testing →](../Lesson-03-Automated-Testing/)

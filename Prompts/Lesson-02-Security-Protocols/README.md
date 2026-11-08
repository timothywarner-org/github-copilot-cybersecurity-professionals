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
2. [JWT Validation Middleware](./ALL-PROMPTS.md#2-jwt-validation-middleware)
3. [OAuth Security Tests](./ALL-PROMPTS.md#3-oauth-security-tests)

### Encryption & Key Management (3 prompts)
4. [AES-256-GCM Encryption with Azure Key Vault](./ALL-PROMPTS.md#4-aes-256-gcm-encryption-with-azure-key-vault)
5. [Password Hashing with bcrypt](./ALL-PROMPTS.md#5-password-hashing-with-bcrypt)
6. [Secret Scanning Validation](../../Demos/Lesson-02-Demo-Runbook.md#demo-23-ghas-secret-scanning-catches-key-leak-3-min)

### API Gateway Security (3 prompts)
7. [Rate Limiting Middleware](./ALL-PROMPTS.md#6-rate-limiting-middleware)
8. [Request Validation Middleware](./ALL-PROMPTS.md#7-request-validation-middleware)
9. [CORS Security Configuration](./ALL-PROMPTS.md#8-cors-security-configuration)

### GHAS Integration
10. [Custom CodeQL Queries for Python](./ALL-PROMPTS.md#9-custom-codeql-queries-for-python)

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

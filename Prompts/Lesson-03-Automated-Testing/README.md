# Lesson 03: Automated Security Testing Prompts

**9 prompts for building comprehensive security testing suites**

## Overview

Build automated security testing covering:
- Security unit tests (OAuth, input validation)
- Fuzz testing frameworks
- SAST integration (CodeQL with custom queries)
- DAST integration (OWASP ZAP)
- Continuous security validation pipelines
- Security metrics and dashboards

**Target Application**: WebGoat (OWASP deliberately insecure Java application)
**Skill Level**: Advanced
**Time to Complete**: 15-20 minutes

---

## Prompts in This Lesson

### Security Unit Tests
1. [OAuth Security Tests](./ALL-PROMPTS.md#1-oauth-security-tests)
2. [Input Validation Security Tests](./ALL-PROMPTS.md#2-input-validation-security-tests)

### Fuzz Testing
3. [Fuzz Testing Framework](./ALL-PROMPTS.md#3-fuzz-testing-framework)
4. [Payload Generation](./ALL-PROMPTS.md#2-input-validation-security-tests)

### SAST Integration
5. [CodeQL Workflow Configuration](./ALL-PROMPTS.md#4-codeql-workflow-configuration)
6. [Custom CodeQL Queries](./ALL-PROMPTS.md#5-custom-codeql-queries)

### DAST Integration
7. [OWASP ZAP Integration](./ALL-PROMPTS.md#6-owasp-zap-dast-integration)

### Continuous Security
8. [Comprehensive Security Pipeline](./ALL-PROMPTS.md#7-comprehensive-security-pipeline)
9. [Security Metrics Dashboard](./ALL-PROMPTS.md#8-security-metrics-dashboard)

---

## Quick Start

```bash
git clone https://github.com/WebGoat/WebGoat.git
cd WebGoat
mvn clean install
java -jar webgoat-server/target/webgoat-server-v8.2.3.jar
```

---

## Key Philosophy

**Security tests validate that ATTACKS FAIL, not that features work.**

A passing security test means:
- ✅ SQL injection was blocked
- ✅ XSS payload was escaped
- ✅ Unauthorized access was denied

A failing security test means:
- ❌ Attack succeeded (vulnerability exists)

---

[View All Prompts →](./ALL-PROMPTS.md) | [← Back to Prompts Home](../README.md)

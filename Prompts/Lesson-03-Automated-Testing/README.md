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
1. [OAuth Security Tests](./01-OAuth-Security-Tests.md)
2. [Input Validation Security Tests](./02-Input-Validation-Tests.md)

### Fuzz Testing
3. [Fuzz Testing Framework](./03-Fuzzing-Framework.md)
4. [Payload Generation](./04-Payload-Generation.md)

### SAST Integration
5. [CodeQL Workflow Configuration](./05-CodeQL-Workflow.md)
6. [Custom CodeQL Queries](./06-CodeQL-Custom-Queries.md)

### DAST Integration
7. [OWASP ZAP Integration](./07-ZAP-DAST-Integration.md)

### Continuous Security
8. [Comprehensive Security Pipeline](./08-Security-Pipeline.md)
9. [Security Metrics Dashboard](./09-Security-Metrics.md)

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

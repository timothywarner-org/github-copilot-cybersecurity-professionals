---
description: "Review the selected code for OWASP Top 10 vulnerabilities and provide remediation guidance"
---

Review the following code for all OWASP Top 10 (2021) vulnerability categories:

1. **A01 Broken Access Control** - Missing or bypassed authorization
2. **A02 Cryptographic Failures** - Weak algorithms, exposed secrets, missing encryption
3. **A03 Injection** - SQL, NoSQL, OS command, LDAP, XSS
4. **A04 Insecure Design** - Missing threat modeling, insecure business logic
5. **A05 Security Misconfiguration** - Default creds, verbose errors, unnecessary features
6. **A06 Vulnerable Components** - Known CVEs, outdated dependencies
7. **A07 Authentication Failures** - Weak passwords, missing MFA, session issues
8. **A08 Integrity Failures** - Insecure deserialization, unsigned updates
9. **A09 Logging Failures** - Missing audit logs, log injection
10. **A10 SSRF** - Unvalidated URL fetching, metadata endpoint access

For each finding, provide:
- The OWASP category and CWE identifier
- Severity rating (CRITICAL, HIGH, MEDIUM, LOW)
- The vulnerable code snippet
- A corrected code snippet with explanation
- A test case to verify the fix

Prioritize findings by severity. If no vulnerabilities are found, confirm which categories were checked and suggest defense-in-depth improvements.

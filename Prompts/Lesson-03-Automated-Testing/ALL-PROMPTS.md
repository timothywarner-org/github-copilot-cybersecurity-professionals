# Lesson 03: All Automated Security Testing Prompts

**Complete collection of 9 prompts for security testing**

---

## 1. OAuth Security Tests

```text
Generate JUnit 5 security tests for OAuth 2.0 authentication that verify:

1. Token Expiration Enforcement:
   - Expired access tokens are rejected with 401
   - Token expiration claims are validated
   - Clock skew tolerance is 5 minutes maximum

2. Token Tampering Detection:
   - Modified JWT signature fails validation
   - Changed claims (sub, aud) are detected
   - Token issued by wrong issuer is rejected

3. Refresh Token Rotation:
   - Refresh tokens are single-use
   - Reusing revoked refresh token fails
   - New refresh token is issued on successful refresh

4. Scope Enforcement:
   - Endpoints enforce required OAuth scopes
   - Insufficient scopes return 403 Forbidden
   - Scope validation happens before business logic

5. Authorization Bypass Prevention:
   - Cannot access other users' resources
   - Tenant isolation is enforced
   - Admin endpoints reject non-admin tokens

Use MockMvc for request testing and include realistic attack payloads.
Add detailed assertions and failure messages for each security control.
```

**Use for**: Testing authentication and authorization
**Customization**: Replace "JUnit 5" with pytest, Jest, PHPUnit; "MockMvc" with your test client

---

## 2. Input Validation Security Tests

```text
Create security tests for WebGoat's input validation that attempt:

1. SQL Injection attacks with payloads from sqlmap
2. XSS attacks (reflected, stored, DOM-based)
3. Path traversal (../../etc/passwd)
4. Command injection (;&|`$())
5. XML External Entity (XXE) attacks
6. Server-Side Request Forgery (SSRF)
7. Oversized payloads (DoS)
8. Special character encoding bypasses

Each test should verify the attack FAILS (meaning security works).
Include attack payload in test name for documentation.
```

**Use for**: Testing input validation and attack prevention
**Customization**: Adapt attack payloads for your application's attack surface

---

## 3. Fuzz Testing Framework

```text
Create a comprehensive fuzzing framework for WebGoat that:

1. Generates SQL injection payloads covering:
   - Union-based attacks
   - Boolean-based blind injection
   - Time-based blind injection
   - Stacked queries
   - Second-order injection

2. Generates XSS payloads covering:
   - Script tags with various encodings
   - Event handlers (onerror, onload, onclick)
   - JavaScript URIs
   - SVG-based XSS
   - Mutation XSS (mXSS)

3. Tests all input points:
   - Query parameters
   - POST body fields
   - HTTP headers (User-Agent, Referer, Cookie)
   - File uploads
   - JSON API inputs

4. Detects vulnerabilities by:
   - Checking for error messages indicating SQL errors
   - Looking for reflected payloads in responses
   - Timing analysis for blind injection
   - Response size anomalies

5. Generates detailed report with:
   - Vulnerable endpoints
   - Attack payloads that succeeded
   - Severity classification
   - OWASP Top 10 mapping

Use Java with RestAssured for HTTP fuzzing.
Implement parallelization for speed.
Save as src/test/java/org/owasp/webgoat/FuzzingFramework.java
```

**Use for**: Discovering edge case vulnerabilities
**Customization**: Replace "Java/RestAssured" with Python/requests, Node/axios, etc.

---

## 4. CodeQL Workflow Configuration

```text
Generate a GitHub Actions workflow for CodeQL scanning that:

1. Scans on every pull request and push to main
2. Uses java-security-extended query suite (comprehensive)
3. Analyzes Java and JavaScript in the repository
4. Fails build on HIGH or CRITICAL severity findings
5. Uploads SARIF results to GitHub Security tab
6. Posts CodeQL findings as PR comments
7. Includes custom CodeQL queries from .github/codeql/queries/

Save as .github/workflows/codeql-analysis.yml
```

**Use for**: Automated static analysis in CI/CD
**Customization**: Replace "java-security-extended" with language-specific query suites

---

## 5. Custom CodeQL Queries

```text
Create a custom CodeQL query for Java that detects:

1. User input flowing to SQL queries without parameterization
2. Hard-coded cryptographic keys or passwords
3. Insecure random number generation (Math.random() instead of SecureRandom)
4. Missing authentication checks on sensitive endpoints
5. Logging of sensitive data (passwords, tokens, SSNs)

Use CodeQL's data flow analysis to track tainted data from sources to sinks.
Include query metadata for severity and precision.
Save as .github/codeql/queries/java-security.ql
```

**Use for**: Organization-specific security rules
**Customization**: Adapt for Python, JavaScript, C#, Go

---

## 6. OWASP ZAP DAST Integration

```text
Generate a GitHub Actions workflow that:

1. Starts WebGoat application in Docker
2. Waits for application health check
3. Runs OWASP ZAP full scan against http://localhost:8080/WebGoat
4. Generates HTML and JSON reports
5. Fails if HIGH severity vulnerabilities found
6. Uploads ZAP reports as artifacts
7. Posts summary to PR comment

Include ZAP automation framework configuration.
Save as .github/workflows/zap-dast-scan.yml
```

**Use for**: Runtime security testing
**Customization**: Replace "WebGoat" with your application; adjust scan rules

---

## 7. Comprehensive Security Pipeline

```text
Generate a GitHub Actions workflow that orchestrates multiple security tools:

1. Runs security unit tests (JUnit)
2. Executes fuzzing framework (parallelized)
3. Performs SAST with CodeQL and Semgrep in parallel
4. Runs DAST with ZAP on staging deployment
5. Checks dependencies with Dependabot
6. Validates secrets aren't committed
7. Generates combined security report
8. Blocks merge if any critical issues found

Use matrix strategy for parallelization.
Set appropriate timeouts for each job.
Cache dependencies for speed.
Save as .github/workflows/security-pipeline.yml
```

**Use for**: Complete security gate in CI/CD
**Customization**: Add/remove tools based on your stack

---

## 8. Security Metrics Dashboard

```text
Create a Python script that:

1. Fetches security metrics from GitHub API:
   - CodeQL alerts (open, fixed, dismissed)
   - Dependabot vulnerabilities by severity
   - Secret scanning findings
   - ZAP DAST results from artifacts

2. Calculates security KPIs:
   - Mean Time to Remediate (MTTR) for vulnerabilities
   - Vulnerability introduction rate (per week)
   - Test coverage for security tests
   - Percentage of PRs blocked by security gates

3. Generates HTML dashboard with charts
4. Updates GitHub Wiki page automatically

Use plotly for visualizations.
Save as scripts/security-dashboard.py
```

**Use for**: Tracking security posture over time
**Customization**: Integrate with your monitoring tools (Datadog, Grafana, etc.)

---

## 9. Parameterized Security Test Suite

```text
Generate parameterized security tests using JUnit 5 @ParameterizedTest that test:

1. 50+ SQL injection payloads from OWASP
2. 30+ XSS payloads covering all attack vectors
3. 20+ path traversal patterns
4. 15+ command injection payloads

Each payload should:
- Be tested against all applicable endpoints
- Include expected behavior (400 Bad Request or escaped output)
- Document the attack technique in test name
- Assert both status code and response content

Use @CsvSource or @MethodSource for payload data.
Organize by OWASP Top 10 category.
```

**Use for**: Comprehensive attack payload coverage
**Customization**: Adapt payload sources for your application

---

## Quick Reference: Testing Strategy

| Test Type | When to Run | Purpose | Prompt # |
|-----------|------------|---------|----------|
| Unit Tests | Every commit | Verify security controls work | 1, 2 |
| Fuzz Tests | Nightly | Find edge cases | 3, 9 |
| SAST (CodeQL) | Every PR | Find code vulnerabilities | 4, 5 |
| DAST (ZAP) | Pre-deployment | Find runtime issues | 6 |
| Full Pipeline | Release candidates | Complete validation | 7 |
| Metrics | Weekly | Track progress | 8 |

---

## Best Practices

### Test Naming
```java
// ✅ Good: Descriptive, includes attack type
@Test
void shouldRejectSQLInjectionInUserIdParameter_UnionAttack() { }

// ❌ Bad: Generic, unclear purpose
@Test
void testSecurity() { }
```

### Assertions
```java
// ✅ Good: Multiple assertions, clear failure messages
assertAll(
    () -> assertEquals(400, response.getStatus(), "Should return 400 Bad Request"),
    () -> assertTrue(response.getBody().contains("Invalid"), "Should indicate validation failure"),
    () -> assertFalse(response.getBody().contains("password"), "Should not leak sensitive data")
);
```

---

[← Back to Lesson 03 Index](./README.md) | [Next: Lesson 04 →](../Lesson-04-Code-Review-Threat-Modeling/)

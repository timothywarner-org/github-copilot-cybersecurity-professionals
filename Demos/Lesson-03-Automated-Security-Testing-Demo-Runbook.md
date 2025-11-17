# Lesson 3: Automated Security Testing with GitHub Copilot

## Demo Runbook (15-20 minutes)

**Target Audience:** Professional developers building comprehensive security testing suites

**Demo Application:** WebGoat (OWASP's deliberately insecure Java application)

**Tools Required:**

- VS Code with GitHub Copilot Enterprise
- GitHub Enterprise Cloud with Advanced Security
- Java 17+ and Maven
- Docker
- OWASP ZAP (for DAST)
- Node.js (for additional tooling)

---

## Prerequisites (Complete before demo - 5 minutes)

### 1. Clone and Setup WebGoat

```bash
git clone https://github.com/WebGoat/WebGoat.git
cd WebGoat
mvn clean install
java -jar webgoat-server/target/webgoat-server-v8.2.3.jar
```

WebGoat will be available at `http://localhost:8080/WebGoat`

### 2. Install Security Testing Tools

```bash
# OWASP ZAP (via Docker)
docker pull zaproxy/zap-stable

# Semgrep for SAST
pip install semgrep

# Jest for JavaScript security tests
npm install -g jest @types/jest
```

### 3. Configure GitHub Actions

Ensure repository has Actions enabled and GHAS is active.

---

## Demo Part 1: Security Unit Tests with Copilot (5 minutes)

### Scenario

Generate comprehensive security unit tests for WebGoat's authentication module to verify security controls work correctly.

### Step 1: Generate OAuth Security Tests

Create `src/test/java/org/owasp/webgoat/AuthSecurityTest.java` and use Copilot:

**Copilot Prompt:**

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

**Expected Outcome:**

Copilot generates comprehensive test class:

```java
@SpringBootTest
@AutoConfigureMockMvc
class AuthSecurityTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @DisplayName("Should reject expired JWT access token")
    void testExpiredTokenRejection() throws Exception {
        // Generate token that expired 1 hour ago
        String expiredToken = JwtTestUtils.createExpiredToken(
            Duration.ofHours(-1)
        );

        mockMvc.perform(get("/api/account")
                .header("Authorization", "Bearer " + expiredToken))
            .andExpect(status().isUnauthorized())
            .andExpect(header().string("WWW-Authenticate",
                containsString("invalid_token")))
            .andExpect(jsonPath("$.error").value("token_expired"));
    }

    @Test
    @DisplayName("Should detect JWT signature tampering")
    void testTokenTamperingDetection() throws Exception {
        // Create valid token then tamper with signature
        String validToken = JwtTestUtils.createValidToken("user123");
        String tamperedToken = validToken.substring(0, validToken.length() - 10) + "TAMPERED==";

        mockMvc.perform(get("/api/account")
                .header("Authorization", "Bearer " + tamperedToken))
            .andExpect(status().isUnauthorized())
            .andExpect(jsonPath("$.error").value("invalid_signature"))
            .andExpect(jsonPath("$.error_description")
                .value(containsString("JWT signature verification failed")));
    }

    @Test
    @DisplayName("Should prevent refresh token reuse (rotation)")
    void testRefreshTokenRotation() throws Exception {
        // Get initial tokens
        String refreshToken = obtainRefreshToken("user123");

        // Use refresh token first time - should succeed
        mockMvc.perform(post("/oauth/token")
                .param("grant_type", "refresh_token")
                .param("refresh_token", refreshToken))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.access_token").exists())
            .andExpect(jsonPath("$.refresh_token").exists());

        // Attempt to reuse same refresh token - should fail
        mockMvc.perform(post("/oauth/token")
                .param("grant_type", "refresh_token")
                .param("refresh_token", refreshToken))
            .andExpect(status().isUnauthorized())
            .andExpect(jsonPath("$.error").value("invalid_grant"))
            .andExpect(jsonPath("$.error_description")
                .value(containsString("Refresh token has been revoked")));
    }

    @Test
    @DisplayName("Should enforce OAuth scope requirements")
    void testScopeEnforcement() throws Exception {
        // Create token with only "read:profile" scope
        String limitedScopeToken = JwtTestUtils.createTokenWithScopes(
            "user123",
            List.of("read:profile")
        );

        // Attempt write operation - should be forbidden
        mockMvc.perform(put("/api/profile")
                .header("Authorization", "Bearer " + limitedScopeToken)
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\": \"Updated Name\"}"))
            .andExpect(status().isForbidden())
            .andExpect(jsonPath("$.error").value("insufficient_scope"))
            .andExpect(jsonPath("$.error_description")
                .value(containsString("write:profile")));
    }

    @Test
    @DisplayName("Should prevent Insecure Direct Object Reference (IDOR)")
    void testIDORPrevention() throws Exception {
        // User A's token
        String userAToken = JwtTestUtils.createTokenForUser("userA", "123");

        // Attempt to access User B's resources
        mockMvc.perform(get("/api/users/456/orders")  // User B's ID
                .header("Authorization", "Bearer " + userAToken))
            .andExpect(status().isForbidden())
            .andExpect(jsonPath("$.error").value("access_denied"))
            .andExpect(jsonPath("$.error_description")
                .value(containsString("not authorized to access this resource")));
    }
}
```

### Step 2: Generate Input Validation Security Tests

**Copilot Prompt:**

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

---

## Demo Part 2: Fuzz Testing Harness (5 minutes)

### Scenario

Build automated fuzzer that generates thousands of malicious inputs to discover edge case vulnerabilities.

### Step 1: Generate Fuzz Testing Framework

**Copilot Prompt:**

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

**Expected Outcome:**

Sophisticated fuzzer that can test thousands of payloads per minute:

```java
public class FuzzingFramework {

    private final List<String> sqlInjectionPayloads = Arrays.asList(
        "' OR '1'='1",
        "' OR '1'='1' --",
        "' OR '1'='1' /*",
        "' UNION SELECT NULL, NULL, NULL --",
        "'; DROP TABLE users; --",
        "1' AND SLEEP(5) --",
        "1' AND (SELECT * FROM (SELECT(SLEEP(5)))a) --"
    );

    private final List<String> xssPayloads = Arrays.asList(
        "<script>alert('XSS')</script>",
        "<img src=x onerror=alert('XSS')>",
        "<svg/onload=alert('XSS')>",
        "javascript:alert('XSS')",
        "<iframe src=\"javascript:alert('XSS')\">",
        "\"><script>alert(String.fromCharCode(88,83,83))</script>"
    );

    @Test
    public void fuzzAllEndpointsForSQLInjection() {
        List<String> endpoints = discoverEndpoints();
        Map<String, List<String>> vulnerabilities = new ConcurrentHashMap<>();

        // Parallel fuzzing for performance
        endpoints.parallelStream().forEach(endpoint -> {
            sqlInjectionPayloads.forEach(payload -> {
                Response response = RestAssured
                    .given()
                        .queryParam("input", payload)
                    .when()
                        .get(endpoint)
                    .then()
                        .extract().response();

                // Detect SQL errors in response
                if (isSQLErrorPresent(response)) {
                    vulnerabilities.computeIfAbsent(endpoint, k -> new ArrayList<>())
                                   .add(payload);
                    logVulnerability(endpoint, payload, "SQL Injection");
                }
            });
        });

        // Fail test if vulnerabilities found
        assertThat(vulnerabilities)
            .as("SQL Injection vulnerabilities detected")
            .isEmpty();
    }

    private boolean isSQLErrorPresent(Response response) {
        String body = response.getBody().asString();
        return body.contains("SQL syntax")
            || body.contains("ORA-")
            || body.contains("MySQL Error")
            || body.contains("pg_query")
            || response.getStatusCode() == 500;
    }
}
```

### Step 2: Run Fuzzer and Analyze Results

```bash
mvn test -Dtest=FuzzingFramework
```

**Expected Outcome:**

Detailed report showing:

```text
Fuzzing Results - WebGoat Security Assessment
=============================================

Endpoints Tested: 47
Total Payloads: 3,450
Duration: 2 minutes 34 seconds

VULNERABILITIES FOUND:

[CRITICAL] SQL Injection in /WebGoat/SqlInjection/attack5a
  Payload: ' UNION SELECT NULL, NULL, version() --
  Evidence: Database version exposed: PostgreSQL 13.3
  OWASP: A03:2021 - Injection
  CWE: CWE-89

[HIGH] Reflected XSS in /WebGoat/CrossSiteScripting/attack2
  Payload: <img src=x onerror=alert('XSS')>
  Evidence: Payload reflected in response without encoding
  OWASP: A03:2021 - Injection (XSS)
  CWE: CWE-79

[MEDIUM] Path Traversal in /WebGoat/PathTraversal/profile-upload
  Payload: ../../../../etc/passwd
  Evidence: File system path exposed in error message
  OWASP: A01:2021 - Broken Access Control
  CWE: CWE-22
```

---

## Demo Part 3: SAST and DAST Integration (6 minutes)

### Step 1: Configure CodeQL for Java Security Scanning

**Copilot Prompt:**

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

**Expected Outcome:**

```yaml
name: CodeQL Security Analysis

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 6 * * 1'  # Weekly Monday 6 AM

jobs:
  analyze:
    name: Analyze with CodeQL
    runs-on: ubuntu-latest
    permissions:
      security-events: write
      actions: read
      contents: read

    strategy:
      fail-fast: false
      matrix:
        language: [ 'java', 'javascript' ]

    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Initialize CodeQL
      uses: github/codeql-action/init@v3
      with:
        languages: ${{ matrix.language }}
        queries: security-extended,+.github/codeql/queries/java-security.ql
        config-file: ./.github/codeql/codeql-config.yml

    - name: Build Java application
      if: matrix.language == 'java'
      run: |
        mvn clean compile -DskipTests

    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v3
      with:
        category: "/language:${{ matrix.language }}"
        upload: true

    - name: Check for critical vulnerabilities
      run: |
        # Fail build if critical/high severity findings exist
        if gh api "/repos/$GITHUB_REPOSITORY/code-scanning/alerts" \
           --jq '[.[] | select(.state=="open" and (.rule.security_severity_level=="critical" or .rule.security_severity_level=="high"))] | length' | grep -v "^0$"; then
          echo "::error::Critical or high severity vulnerabilities found"
          exit 1
        fi
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Step 2: Write Custom CodeQL Query

**Copilot Prompt:**

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

### Step 3: Integrate OWASP ZAP for DAST

**Copilot Prompt:**

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

**Expected Outcome:**

Full DAST pipeline that discovers runtime vulnerabilities:

```yaml
name: OWASP ZAP DAST Scan

on:
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 2 * * *'  # Nightly at 2 AM

jobs:
  zap_scan:
    runs-on: ubuntu-latest
    name: Dynamic Application Security Testing

    steps:
    - uses: actions/checkout@v4

    - name: Start WebGoat
      run: |
        docker run -d -p 8080:8080 --name webgoat webgoat/webgoat:latest
        # Wait for application to be ready
        timeout 60 bash -c 'until curl -f http://localhost:8080/WebGoat/login; do sleep 2; done'

    - name: Run ZAP Full Scan
      uses: zaproxy/action-full-scan@v0.9.0
      with:
        target: 'http://localhost:8080/WebGoat'
        rules_file_name: '.zap/rules.tsv'
        cmd_options: '-a -j -l WARN -z "-config api.disablekey=true"'
        fail_action: true
        allow_issue_writing: true

    - name: Upload ZAP Reports
      if: always()
      uses: actions/upload-artifact@v4
      with:
        name: zap-reports
        path: |
          report_html.html
          report_json.json

    - name: Analyze ZAP Results
      run: |
        HIGH_COUNT=$(jq '[.site[].alerts[] | select(.riskcode == "3")] | length' report_json.json)
        CRITICAL_COUNT=$(jq '[.site[].alerts[] | select(.riskcode == "4")] | length' report_json.json)

        echo "::notice::ZAP found $CRITICAL_COUNT critical and $HIGH_COUNT high severity issues"

        if [ "$CRITICAL_COUNT" -gt "0" ] || [ "$HIGH_COUNT" -gt "0" ]; then
          echo "::error::DAST scan failed - critical/high severity vulnerabilities detected"
          exit 1
        fi
```

---

## Demo Part 4: Continuous Security Validation Pipeline (3 minutes)

### Step 1: Create Comprehensive Security Gate

**Copilot Prompt:**

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

**Expected Outcome:**

Multi-stage pipeline that completes in under 15 minutes with parallel execution.

### Step 2: Generate Security Metrics Dashboard

**Copilot Prompt:**

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

---

## Validation & Key Takeaways (1 minute)

### What We Demonstrated

1. **Security Unit Tests**: Comprehensive OAuth and input validation tests that verify attacks fail
2. **Fuzz Testing**: Automated fuzzer generating thousands of malicious payloads
3. **SAST Integration**: CodeQL with custom queries detecting Java-specific vulnerabilities
4. **DAST Integration**: OWASP ZAP full scan in CI/CD pipeline
5. **Continuous Validation**: Multi-stage security pipeline running on every PR
6. **Metrics & Reporting**: Automated security dashboards tracking progress

### Prompting Best Practices

- **Request complete test suites**, not individual tests
- **Specify attack categories** (OWASP Top 10, CWE)
- **Ask for both positive and negative tests**
- **Demand realistic payloads** from known exploit databases
- **Request parallelization** for performance

### Testing Philosophy

Remember: Security tests validate that **attacks fail**, not that features work. A passing security test means the attack was blocked.

---

## Next Steps

1. Enable CodeQL on your repositories
2. Build custom fuzzing for your APIs
3. Integrate OWASP ZAP into staging deployments
4. Create security test libraries for reuse
5. Move to Lesson 4: Security Code Review & Threat Modeling

---

## Troubleshooting

**Issue**: CodeQL failing on build errors

- **Solution**: Ensure Java application compiles successfully first
- Use `mvn compile -DskipTests` to verify
- Check CodeQL logs for specific build failures

**Issue**: ZAP scan taking too long

- **Solution**: Use ZAP baseline scan instead of full scan for PR validation
- Full scans should run nightly, not on every PR
- Configure ZAP to skip slow checks in `.zap/rules.tsv`

**Issue**: Fuzzer generating too many false positives

- **Solution**: Refine detection logic to reduce false positives
- Add baseline comparison (test against known-good responses)
- Tune payload selection based on application stack

---

## Additional Resources

- WebGoat: <https://github.com/WebGoat/WebGoat>
- OWASP ZAP: <https://www.zaproxy.org/>
- CodeQL Documentation: <https://codeql.github.com/>
- Semgrep Rules: <https://semgrep.dev/explore>
- PayloadsAllTheThings: <https://github.com/swisskyrepo/PayloadsAllTheThings>

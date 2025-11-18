# Lesson 04: All Code Review & Threat Modeling Prompts

**Complete collection of 9 prompts for security analysis and threat modeling**

---

## 1. Set Architectural Context

```text
I'm reviewing a Node.js/Express application with the following architecture:
- Frontend: EJS templates with client-side JavaScript
- Backend: Express.js with MongoDB
- Authentication: Passport.js with local strategy
- Session Management: express-session with MongoDB store
- Payment Processing: Stripe integration
- File Uploads: multer middleware

Threat Model Context:
- Internet-facing application
- Handles PII (names, addresses, SSNs)
- Processes credit card payments
- Multi-tenant SaaS architecture

Based on this context, I'll be asking security review questions about specific files.
Acknowledge this architecture context and summarize the top 5 security concerns.
```

**Use for**: Establishing context before detailed analysis
**Customization**: Replace with your actual architecture, data types, and deployment model

---

## 2. Deep Security Audit

```text
@workspace #file:session.js

Perform a comprehensive security audit of this authentication code.
Analyze for:

1. Password Storage: bcrypt usage, salt rounds, comparison timing attacks
2. Session Management: fixation, hijacking, secure cookie flags
3. Brute Force Protection: rate limiting, account lockout
4. Credential Validation: input sanitization, parameter pollution
5. Error Handling: information disclosure in auth failures

For each finding, provide:
- Exact line number
- Vulnerability description
- OWASP classification
- CWE reference
- Proof of concept exploit
- Remediation code snippet
```

**Use for**: Comprehensive file-level security review
**Customization**: Replace "session.js" with your file; adjust security focus areas

---

## 3. Conversational Follow-up Analysis

```text
For the session fixation vulnerability you found, show me:
1. A complete proof-of-concept exploit script
2. How this could be combined with XSS for maximum impact
3. Detection methods (what logs would show the attack)
4. Defensive code that prevents this entire class of vulnerability

Use the architecture context I provided earlier about this being a multi-tenant SaaS app.
```

**Use for**: Deep-dive into specific findings
**Customization**: Reference earlier context; ask about specific vulnerability

---

## 4. Generate Payment Flow Diagram

```text
@workspace

Analyze the payment processing workflow in this application by examining:
- app/routes/payment.js
- app/models/allocation.js
- Any Stripe API integrations

Generate a Mermaid sequence diagram showing:
1. User initiates payment
2. Payment validation
3. Stripe API interaction
4. Database updates
5. Confirmation flow

Include all trust boundaries (client, server, external API, database).
```

**Use for**: Visualizing data flows for threat modeling
**Customization**: Replace files and service names; request PlantUML instead of Mermaid

---

## 5. STRIDE Threat Model Analysis

```text
Using the payment flow diagram you created, perform a comprehensive STRIDE threat model:

S - Spoofing: How could an attacker impersonate users or external services?
T - Tampering: What data could be modified in transit or at rest?
R - Repudiation: What actions lack audit trails?
I - Information Disclosure: What sensitive data could leak?
D - Denial of Service: What could make the system unavailable?
E - Elevation of Privilege: How could users gain unauthorized access?

For each threat category:
1. List specific threats (minimum 3 per category)
2. Rate likelihood (High/Medium/Low) and impact (High/Medium/Low)
3. Identify affected components
4. Propose mitigations with implementation priority
5. Map to OWASP Top 10 and CWE

Format as a comprehensive markdown table.
```

**Use for**: Systematic threat identification
**Customization**: Can request specific STRIDE categories or focus areas

---

## 6. Mitigation Implementation Plan

```text
Based on the STRIDE analysis, generate a prioritized implementation plan:

1. Group mitigations by implementation effort (Quick wins, Medium effort, Large projects)
2. Create GitHub Issues for each mitigation with:
   - Detailed description
   - Acceptance criteria
   - Implementation guidance
   - Security test requirements
3. Generate code snippets for top 3 highest priority mitigations
4. Create a security roadmap (timeline for implementation)

Export as markdown suitable for pasting into GitHub Project.
```

**Use for**: Converting threat model to actionable tasks
**Customization**: Request Jira format, Azure DevOps work items, etc.

---

## 7. GHAS API Compliance Report

```text
Create a Node.js script that:

1. Uses GitHub GraphQL API to fetch:
   - All CodeQL code scanning alerts
   - Secret scanning findings
   - Dependabot vulnerability alerts
   - Pull request security checks history

2. Aggregates data by:
   - Severity (critical, high, medium, low)
   - OWASP Top 10 category
   - CWE classification
   - Component/file path
   - Age (time since first detected)

3. Generates three reports:
   a) Executive summary (1-page PDF)
   b) Technical detail (full OWASP mapping)
   c) Trend analysis (week-over-week improvements)

Use the @octokit/graphql library and export reports in Markdown and JSON.
Save as scripts/generate-compliance-report.js
```

**Use for**: Automated security reporting
**Customization**: Replace "Node.js" with Python, Go, etc.; adjust report formats

---

## 8. Automated Report Generation Workflow

```text
Create a GitHub Actions workflow that:

1. Runs weekly (Monday 9 AM) and on-demand via workflow_dispatch
2. Executes the compliance report script
3. Generates reports in multiple formats (Markdown, HTML, PDF)
4. Commits reports to docs/security-reports/ with timestamp
5. Creates GitHub Wiki page with latest report
6. Posts summary to Slack channel #security
7. Opens GitHub Issue if critical vulnerabilities exceed threshold

Save as .github/workflows/weekly-security-report.yml
```

**Use for**: Continuous compliance monitoring
**Customization**: Change schedule, notification channels, report destinations

---

## 9. CVE Exploitability Analysis

```text
@github Analyze Dependabot alert #42 (lodash prototype pollution CVE-2024-XXXXX):

Context about our application:
- We use lodash only for server-side data transformation
- Input data comes from authenticated admin users only
- Data is validated before lodash processing
- Application runs in isolated Kubernetes pods

Questions:
1. Is this CVE exploitable given our specific usage pattern?
2. What would an attacker need to compromise to exploit this?
3. Are there compensating controls that mitigate the risk?
4. What's the recommended priority (Critical/High/Medium/Low)?
5. Generate an upgrade plan considering breaking changes in latest lodash version
```

**Use for**: Intelligent vulnerability triage
**Customization**: Replace CVE details and application context; adjust questions

---

## Quick Reference: Code Review Workflow

| Phase | Purpose | Prompts |
|-------|---------|---------|
| **Context** | Establish architecture | 1 |
| **Analyze** | Find vulnerabilities | 2 |
| **Deep Dive** | Understand exploits | 3 |
| **Model Threats** | Identify attack scenarios | 4-5 |
| **Plan** | Create remediation roadmap | 6 |
| **Report** | Track compliance | 7-8 |
| **Triage** | Prioritize dependencies | 9 |

---

## STRIDE Threat Categories Explained

### Spoofing Identity
- Impersonating users or services
- Token theft, session hijacking
- **Mitigations**: MFA, certificate pinning, mutual TLS

### Tampering with Data
- Modifying data in transit or at rest
- Man-in-the-middle attacks
- **Mitigations**: HTTPS, integrity checks, digital signatures

### Repudiation
- Denying actions taken
- Lack of audit trails
- **Mitigations**: Logging, non-repudiation, digital signatures

### Information Disclosure
- Exposing confidential data
- Data leaks, side channels
- **Mitigations**: Encryption, access controls, sanitization

### Denial of Service
- Making system unavailable
- Resource exhaustion
- **Mitigations**: Rate limiting, resource quotas, DDoS protection

### Elevation of Privilege
- Gaining unauthorized access
- Privilege escalation
- **Mitigations**: Least privilege, RBAC, authorization checks

---

## Advanced Techniques

### Multi-Turn Analysis

**Turn 1**: Broad scan
```text
@workspace Scan all authentication-related files for security issues.
```

**Turn 2**: Focus on finding
```text
For the password hashing issue in auth.js line 42, explain the specific attack.
```

**Turn 3**: Generate fix
```text
Show me the secure refactored code with bcrypt work factor 12.
```

**Turn 4**: Create test
```text
Generate a test that verifies the bcrypt implementation is secure.
```

### Context Layering

Build context progressively:
1. **Architecture context** (prompt 1)
2. **File-specific context** (@workspace #file:path)
3. **Finding-specific context** (reference earlier conversation)
4. **Remediation context** (security standards, compliance requirements)

---

[← Back to Lesson 04 Index](./README.md) | [Next: Lesson 05 →](../Lesson-05-Compliance/)

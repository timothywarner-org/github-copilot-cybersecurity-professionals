# GitHub Copilot Security Prompts Library

**Professional-grade Copilot prompts for cybersecurity professionals**

This library contains 43+ production-ready GitHub Copilot prompts extracted from the MS Press video course "GitHub Copilot for Cybersecurity Professionals." Each prompt is copy-paste ready, well-documented, and designed for real-world security work.

## 📚 Quick Navigation

### By Use Case

| Use Case | Prompts | Location |
|----------|---------|----------|
| **Finding Vulnerabilities** | SQL Injection, XSS, IDOR detection | [Lesson 01](./Lesson-01-Vulnerability-Detection/) |
| **Implementing Auth** | OAuth 2.0, JWT, PKCE | [Lesson 02](./Lesson-02-Security-Protocols/) |
| **Encryption & Keys** | AES-GCM, Key Vault, Password Hashing | [Lesson 02](./Lesson-02-Security-Protocols/) |
| **Security Testing** | Unit tests, Fuzz testing, SAST/DAST | [Lesson 03](./Lesson-03-Automated-Testing/) |
| **Code Review** | Interactive analysis, STRIDE modeling | [Lesson 04](./Lesson-04-Code-Review-Threat-Modeling/) |
| **Compliance** | CIS Benchmarks, STIG, IaC Security | [Lesson 05](./Lesson-05-Compliance/) |

### By Security Domain

- **🔍 Vulnerability Detection**: [11 prompts](./Lesson-01-Vulnerability-Detection/)
- **🔐 Authentication & Encryption**: [9 prompts](./Lesson-02-Security-Protocols/)
- **✅ Security Testing**: [9 prompts](./Lesson-03-Automated-Testing/)
- **👁️ Code Review & Threat Modeling**: [9 prompts](./Lesson-04-Code-Review-Threat-Modeling/)
- **📋 Compliance & IaC**: [8 prompts](./Lesson-05-Compliance/)

---

## 🚀 How to Use This Library

### 1. Choose Your Prompt

Browse by lesson or use case to find the prompt you need.

### 2. Copy the Prompt

Each prompt file contains:
- **Exact prompt text** (in code blocks) - ready to copy
- **Context** - when to use this prompt
- **Expected output** - what Copilot should generate
- **Customization guidance** - how to adapt for your needs
- **Related prompts** - cross-references to similar prompts

### 3. Customize for Your Environment

Replace placeholder values:
- Language/framework (e.g., "Node.js" → "Python/Django")
- File paths (e.g., "app/routes/auth.js" → your actual file)
- Standards (e.g., "CIS Azure" → "CIS AWS" or "NIST 800-53")
- Tech stack specifics (e.g., "MongoDB" → "PostgreSQL")

### 4. Iterate and Refine

- Start with the base prompt
- Review Copilot's output
- Use follow-up prompts to drill deeper
- Validate generated code before production use

---

## 💡 Tips for Effective Copilot Prompting

### Be Specific About Context

❌ **Generic**: "Find vulnerabilities"
✅ **Specific**: "@workspace Analyze this file for SQL injection vulnerabilities. Focus on database query construction and user input handling."

### Request Security Requirements Explicitly

❌ **Vague**: "Create authentication"
✅ **Detailed**: "Generate OAuth 2.0 with PKCE support, JWT with RS256, token expiration (15 min access, 7 day refresh)"

### Ask for Actionable Output

❌ **Abstract**: "Review security"
✅ **Actionable**: "Provide line numbers, OWASP classification, CWE reference, and remediation code snippets"

### Demand Testing

❌ **Code only**: "Fix this SQL injection"
✅ **Code + tests**: "Fix this SQL injection AND generate Jest tests that verify the attack fails"

### Use Workspace Context

- **@workspace**: Provides full codebase awareness
- **#file:path**: References specific files
- **@github**: Queries GHAS alerts directly (2025 feature)

### Specify Standards and Frameworks

Always mention:
- Compliance frameworks (CIS, NIST, PCI-DSS, OWASP)
- Security standards (RFC 6749, PKCE RFC 7636)
- Language versions (Python 3.10+, Node 18+)
- Libraries (cryptography 42+, bcrypt 5+)

### Chain Prompts for Complex Tasks

1. **Discover**: "Analyze for vulnerabilities"
2. **Understand**: "Show me attack payloads for this vulnerability"
3. **Fix**: "Refactor with secure code patterns"
4. **Test**: "Generate security tests"
5. **Integrate**: "Create CI/CD workflow to prevent regression"

---

## 📖 Prompt Library Contents

### Lesson 01: Vulnerability Detection (11 prompts)

**SQL Injection Detection**
- Analyze code for SQL injection
- Generate attack payloads
- Refactor with parameterized queries
- Generate security unit tests

**XSS Detection & Prevention**
- Scan templates for XSS vulnerabilities
- Demonstrate attack vectors
- Implement output encoding and CSP

**Custom Security Scanning**
- Build business logic vulnerability scanner
- Integrate scanner into CI/CD
- Enable CodeQL scanning

[View all Lesson 01 prompts →](./Lesson-01-Vulnerability-Detection/)

---

### Lesson 02: Security Protocols (9 prompts)

**OAuth 2.0 & Authentication**
- Generate OAuth 2.0 server with PKCE
- Create JWT validation middleware
- Generate security tests for OAuth

**Encryption & Key Management**
- Implement AES-256-GCM encryption
- Integrate Azure Key Vault
- Generate password hashing utilities

**API Gateway Security**
- Create rate limiting middleware
- Generate request validation
- Configure CORS security

**GHAS Integration**
- Generate CodeQL custom queries

[View all Lesson 02 prompts →](./Lesson-02-Security-Protocols/)

---

### Lesson 03: Automated Security Testing (9 prompts)

**Security Unit Tests**
- Generate OAuth security tests
- Create input validation tests

**Fuzz Testing**
- Build fuzzing framework
- Generate attack payloads

**SAST & DAST Integration**
- Configure CodeQL workflow
- Create custom CodeQL queries
- Integrate OWASP ZAP scanning

**Continuous Security**
- Create comprehensive security pipeline
- Generate security metrics dashboard

[View all Lesson 03 prompts →](./Lesson-03-Automated-Testing/)

---

### Lesson 04: Code Review & Threat Modeling (9 prompts)

**Interactive Code Review**
- Set architectural context
- Perform deep security audit
- Conversational follow-up analysis

**STRIDE Threat Modeling**
- Generate flow diagrams
- Perform STRIDE analysis
- Create mitigation implementation plan

**Compliance Reporting**
- Query GHAS API for findings
- Automate report generation

**Dependency Analysis**
- Analyze CVE exploitability
- Auto-generate upgrade PRs

[View all Lesson 04 prompts →](./Lesson-04-Code-Review-Threat-Modeling/)

---

### Lesson 05: Compliance & Configuration (8 prompts)

**Infrastructure-as-Code Security**
- Audit Terraform for CIS compliance
- Generate secure IaC modules

**CIS Benchmark Automation**
- Generate validation scripts
- Auto-remediate findings

**STIG Compliance**
- Create PowerShell DSC configurations
- Integrate into CI/CD

**Incident Response**
- Generate IR playbooks
- Automate containment scripts

[View all Lesson 05 prompts →](./Lesson-05-Compliance/)

---

## 🎯 Best Practices Summary

### Context is King
Provide architecture details, tech stack, deployment model, and threat context.

### Security-First Framing
Start prompts with security objectives, not just functional requirements.

### Validate Everything
Always validate Copilot-generated code with:
- Security tests
- Manual review
- SAST/DAST scanning
- Peer review

### Build a Prompting Workflow
1. **Analysis** → Understand vulnerability
2. **Remediation** → Fix with secure patterns
3. **Testing** → Verify attacks fail
4. **Documentation** → Explain security controls
5. **Automation** → Prevent regression

### Iterate Based on Responses
- If output is generic, add more context
- If output is incorrect, provide corrective examples
- If output is incomplete, ask for specific additions

---

## 🔗 Cross-References

### Finding → Fixing → Testing Workflow

1. **Find**: [SQL Injection Detection](./Lesson-01-Vulnerability-Detection/SQL-Injection-Detection.md)
2. **Fix**: [Parameterized Queries Remediation](./Lesson-01-Vulnerability-Detection/SQL-Injection-Remediation.md)
3. **Test**: [Security Unit Tests for SQL Injection](./Lesson-03-Automated-Testing/Security-Unit-Tests.md)
4. **Prevent**: [CodeQL Custom Queries](./Lesson-03-Automated-Testing/CodeQL-Custom-Queries.md)

### Build → Secure → Deploy Workflow

1. **Build**: [OAuth 2.0 Implementation](./Lesson-02-Security-Protocols/OAuth-PKCE-Implementation.md)
2. **Secure**: [JWT Validation Middleware](./Lesson-02-Security-Protocols/JWT-Validation-Middleware.md)
3. **Test**: [OAuth Security Tests](./Lesson-02-Security-Protocols/OAuth-Security-Tests.md)
4. **Review**: [Authentication Audit](./Lesson-04-Code-Review-Threat-Modeling/Authentication-Security-Audit.md)

---

## 📊 Metrics: What to Track

When using these prompts in your organization, track:

- **Prompt Effectiveness**: Time saved vs. manual implementation
- **Code Quality**: SAST findings before/after using prompts
- **Security Coverage**: % of OWASP Top 10 addressed
- **Learning Impact**: Team security knowledge improvement
- **Velocity**: Security features delivered per sprint

---

## 🤝 Contributing

These prompts are designed for the MS Press video course. To customize for your organization:

1. Fork the repository
2. Customize prompts for your tech stack
3. Add organization-specific standards
4. Share learnings with your team

---

## 📚 Additional Resources

- **GitHub Copilot Documentation**: https://docs.github.com/copilot
- **OWASP Top 10**: https://owasp.org/Top10/
- **CWE Top 25**: https://cwe.mitre.org/top25/
- **GitHub Advanced Security**: https://docs.github.com/code-security
- **Copilot Chat Cookbook**: https://docs.github.com/copilot/copilot-chat-cookbook

---

## 📝 License & Usage

These prompts are part of the MS Press video course "GitHub Copilot for Cybersecurity Professionals."

**For course participants**: Use freely in your professional work.
**For organizations**: Customize and adapt for internal use.
**Production usage**: Always validate generated code before deployment.

---

**⚠️ Security Notice**: AI-generated code should always be reviewed by security professionals before production deployment. These prompts are tools to accelerate secure development, not replacements for security expertise.

---

**Last Updated**: 2025-11-18
**Course**: GitHub Copilot for Cybersecurity Professionals (MS Press)
**Prompts**: 43 production-ready security prompts

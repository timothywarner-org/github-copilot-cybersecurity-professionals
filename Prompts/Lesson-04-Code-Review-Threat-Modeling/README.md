# Lesson 04: Code Review & Threat Modeling Prompts

**9 prompts for security code review, threat analysis, and compliance reporting**

## Overview

Use GitHub Copilot for:
- Interactive security code reviews with architectural context
- STRIDE threat modeling with diagrams
- Automated compliance reporting (OWASP, CWE, NIST)
- Intelligent CVE exploitability analysis
- Auto-generated remediation pull requests

**Target Application**: NodeGoat (Node.js/Express with complex business logic)
**Skill Level**: Advanced/Expert
**Time to Complete**: 15-20 minutes

---

## Prompts in This Lesson

### Interactive Code Review (3 prompts)
1. [Set Architectural Context](./01-Architecture-Context.md)
2. [Deep Security Audit](./02-Security-Audit.md)
3. [Conversational Follow-up Analysis](./03-Follow-up-Analysis.md)

### STRIDE Threat Modeling (3 prompts)
4. [Generate Payment Flow Diagram](./04-Flow-Diagram.md)
5. [STRIDE Analysis](./05-STRIDE-Analysis.md)
6. [Mitigation Implementation Plan](./06-Mitigation-Plan.md)

### Compliance Reporting (2 prompts)
7. [GHAS API Compliance Report](./07-Compliance-Report.md)
8. [Automated Report Generation](./08-Report-Automation.md)

### Dependency Analysis (1 prompt)
9. [CVE Exploitability Analysis](./09-CVE-Analysis.md)

---

## Key Concepts

### Architectural Context
Provide detailed context upfront:
- Tech stack (frameworks, databases, cloud services)
- Data sensitivity (PII, PCI, PHI)
- Deployment model (SaaS, on-prem, hybrid)
- Threat landscape (internet-facing, internal-only)

### Conversational Refinement
Use multi-turn conversations:
1. **Discover**: Initial broad security scan
2. **Understand**: Drill into specific findings
3. **Exploit**: Analyze attack scenarios
4. **Fix**: Generate remediation
5. **Validate**: Review and test

---

[View All Prompts →](./ALL-PROMPTS.md) | [← Back to Prompts Home](../README.md)

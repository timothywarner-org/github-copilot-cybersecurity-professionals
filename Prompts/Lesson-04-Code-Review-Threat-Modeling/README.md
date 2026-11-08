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
1. [Set Architectural Context](./ALL-PROMPTS.md#1-set-architectural-context)
2. [Deep Security Audit](./ALL-PROMPTS.md#2-deep-security-audit)
3. [Conversational Follow-up Analysis](./ALL-PROMPTS.md#3-conversational-follow-up-analysis)

### STRIDE Threat Modeling (3 prompts)
4. [Generate Payment Flow Diagram](./ALL-PROMPTS.md#4-generate-payment-flow-diagram)
5. [STRIDE Analysis](./ALL-PROMPTS.md#5-stride-threat-model-analysis)
6. [Mitigation Implementation Plan](./ALL-PROMPTS.md#6-mitigation-implementation-plan)

### Compliance Reporting (2 prompts)
7. [GHAS API Compliance Report](./ALL-PROMPTS.md#7-ghas-api-compliance-report)
8. [Automated Report Generation](./ALL-PROMPTS.md#8-automated-report-generation-workflow)

### Dependency Analysis (1 prompt)
9. [CVE Exploitability Analysis](./ALL-PROMPTS.md#9-cve-exploitability-analysis)

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

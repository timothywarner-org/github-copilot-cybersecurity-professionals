# GitHub Copilot for Cybersecurity Professionals - AI Agent Instructions

## Project Overview

This is a **professional training course** teaching cybersecurity specialists how to use GitHub Copilot and GitHub Advanced Security (GHAS) for security workflows. The repository contains lesson materials, demo runbooks, and PowerPoint presentations for a 5-module course.

**Course URL**: `timw.info/copilot-security`
**Instructor**: Tim Warner
**Target Audience**: Security professionals adopting AI-assisted security development

## Repository Structure

```
Demos/          # Lesson runbooks and course documentation
├── lesson-01-demo-runbook.md              # Vulnerability Detection
├── lesson-02-demo-runbook-v2.md           # Security Protocols
├── Lesson-03-Demo-Runbook-PROPER.md       # Automated Testing
├── copilot-cybersecurity-lesson-04-*.md   # Code Review & Threat Modeling
├── mapping-document.md                     # Repo-to-module mapping
Docs/           # Supporting documentation (Word docs)
PPTs/           # PowerPoint presentation files (.pptx)
```

## Course Module Structure (5 Lessons @ 40 min each)

1. **Lesson 1**: Vulnerability Detection with Copilot (SQL injection, XSS, custom scanners)
2. **Lesson 2**: Implement Security Protocols (auth, encryption, zero-trust IaC)
3. **Lesson 3**: Automated Security Testing (unit tests, fuzzing, SAST/DAST, CI/CD)
4. **Lesson 4**: Security Code Review, Threat Modeling, Auditing (STRIDE, compliance, linters)
5. **Lesson 5**: Compliance, Incident Response, Config Management (CIS, NIST, STIG)

## Content Patterns & Conventions

### Demo Runbook Structure

All lesson runbooks follow this template:

- **Pre-Demo Environment Setup**: Required tools, repository structure, checklist
- **Demo Flows (4 demos @ 10 min each)**: Step-by-step instructions with talking points
- **Teaching Points**: Key concepts to emphasize during demos
- **Key Messages**: Course takeaways and "force multiplier" quotes
- **Common Pitfalls**: Technical/timing/teaching issues to avoid
- **Demo Preparation Checklist**: 24hr/1hr/immediate-before tasks

### PowerPoint Slide Structure (FRAMER Methodology)

Speaker notes use **FRAMER** framework:

- **F**rame: Hook statement that grabs attention
- **R**elate: Connect to real-world security scenarios
- **A**pply: Practical application with enterprise examples (Contoso, Wide World Importers)
- **M**etaphor: Analogies for complex security concepts
- **E**xpand: Detailed bullet-by-bullet explanations
- **R**einforce: Pro tips from real security experience

**Slide Conventions**:

- Titles in sentence case (Microsoft standard)
- "Title and content" layout consistently
- Teaching content in full sentences (no fragments in speaker notes)
- Every slide includes repository URL: `timw.info/copilot-security`

### Technology Stack Used in Demos

- **Languages**: JavaScript/Node.js (primary), TypeScript, Python, Terraform
- **Tools**: VS Code, GitHub Copilot, GitHub Copilot Chat, GHAS (CodeQL, Dependabot)
- **Security**: Semgrep, OWASP ZAP, Azure CLI, ESLint security plugins
- **Vulnerable Apps**: NodeGoat (OWASP), WebGoat, TerraGoat (Bridgecrew)

## Writing Style Guidelines

### Tim Warner's Voice

- **First-principles explanations** using Feynman technique
- **Real-world breach examples** with specific company names and outcomes
- **Skeptical optimism** about AI capabilities - show both power and limitations
- **Practical ROI focus** - metrics, time savings, force multiplier quotes
- **Security professional tone** - technical depth without patronizing

### Example "Force Multiplier" Quotes

> "Security teams using AI-assisted review analyze 10x more code without 10x more people."

> "Making Copilot your security tool instead of your security problem."

### Copilot Prompt Templates

When documenting Copilot interactions, use this format:

```markdown
**Copilot Chat Prompt:**
```

Analyze this Express.js API for SQL injection vulnerabilities.
Focus on: [specific security requirements]
Output: [desired format]

```

**Expected Response:** [What Copilot should identify]
**Teaching Point:** [Security lesson to emphasize]
```

## Working with Course Materials

### When Editing Demo Runbooks

- **Maintain 40-minute total runtime** (typically 4 demos @ 10 min each)
- Include **exact terminal commands** and expected output
- Add **Copilot prompts** verbatim for reproducibility
- Document **common issues** and troubleshooting steps
- Provide **backup screenshots** for Copilot outputs (responses may vary)

### When Creating New Lessons

1. Reference `mapping-document.md` for vulnerable app assignments
2. Follow existing runbook structure (see lesson-01 or lesson-04)
3. Include prerequisites, environment setup, and validation steps
4. Map demos to OWASP Top 10 / CWE patterns where applicable
5. Add GitHub Actions workflows for CI/CD integration examples

### Quality Assurance Checklist

Before finalizing any lesson:

- [ ] All code samples tested and validated
- [ ] Copilot prompts reproduce expected results
- [ ] Security content aligns with OWASP/CWE/NIST frameworks
- [ ] Time allocations sum to 40 minutes
- [ ] Repository URL (`timw.info/copilot-security`) mentioned
- [ ] Pro tips based on real-world experience included
- [ ] Success metrics and key messages defined

## Security Content Standards

### Vulnerability Coverage

Demos must demonstrate **detection + remediation + testing** workflow:

1. Show vulnerable code pattern with Copilot analysis
2. Generate secure alternative using Copilot
3. Create security tests to prevent regression
4. Integrate into CI/CD for continuous validation

### GHAS Integration

All security workflows should demonstrate:

- **CodeQL** for static application security testing (SAST)
- **Dependabot** for dependency vulnerability scanning
- **Secret scanning** for credential leak prevention
- **Security policies** for branch protection and deployment gates

### Compliance Frameworks

Map findings to industry standards:

- **OWASP Top 10** (primary reference)
- **CWE** (Common Weakness Enumeration) identifiers
- **NIST** Cybersecurity Framework (where applicable)
- **CIS Benchmarks** (for IaC and cloud config)

## File Naming Conventions

- Demo runbooks: `lesson-XX-demo-runbook[-variant].md`
- PowerPoint files: `copilot-cybersecurity-lesson-XX[-suffix].pptx`
- Summary documents: `copilot-cybersecurity-lesson-XX-SUMMARY.md`
- Supporting docs: Descriptive names with context (e.g., `mapping-document.md`)

## Key Resources Referenced

**External Vulnerable Applications**:

- NodeGoat: `https://github.com/OWASP/NodeGoat` (Node.js web vulnerabilities)
- WebGoat: `https://github.com/WebGoat/WebGoat` (Java/Spring Boot enterprise patterns)
- TerraGoat: `https://github.com/bridgecrewio/terragoat` (IaC cloud misconfigurations)

**Course Repository**: `timw.info/copilot-security` (always include in materials)

## Common Pitfalls to Avoid

1. **Don't assume Copilot responses are deterministic** - always test prompts and document expected output
2. **Include architectural context in prompts** - generic prompts yield generic (often incorrect) security advice
3. **Show Copilot limitations** - builds appropriate skepticism in security professionals
4. **Validate all generated code** - Copilot provides scaffolding, humans confirm correctness
5. **Keep demos within time allocations** - 10 minutes per demo is firm (course runtime = 40 min)

## When Assisting with This Codebase

- **Editing runbooks**: Follow existing structure, maintain time constraints, test all commands
- **Creating slides**: Use FRAMER methodology, include repository URL, add pro tips
- **Security content**: Map to OWASP/CWE, show vulnerable→secure→tested workflow
- **Copilot examples**: Provide exact prompts, expected responses, and teaching points
- **Documentation**: Use Tim's voice (first-principles, real-world examples, metrics-focused)

# GitHub Copilot instructions: Copilot for Cybersecurity Professionals

## Project overview

This repository supports the Microsoft Press video course **GitHub Copilot for Cybersecurity Professionals**. Learners use GitHub Copilot and GitHub Advanced Security (GHAS) to detect, fix, and test security problems in real code.

- **This repo:** slide PDFs (`PPTs/`), demo runbooks (`Demos/`), the security prompt library (`Prompts/`), and intentionally vulnerable practice apps (`vulnerable-apps/`).
- **Demo code:** each lesson's demo files live in the companion repo [timothywarner-org/github-security-testbed](https://github.com/timothywarner-org/github-security-testbed), in folders named `lesson-01` through `lesson-05`.

## Security context

This is a cybersecurity training workspace, so review and generate code through a security-first lens:

- Treat every code snippet as a potential security teaching example.
- Reference the OWASP Top 10, CWE Top 25, or NIST controls when you discuss a vulnerability.
- Follow the course's detect, remediate, and test pattern: find the flaw, write the secure version, then write a test that keeps it fixed.
- Flag insecure patterns even in comments, documentation, and sample code.
- Code in `vulnerable-apps/` is intentionally insecure for training. Explain its flaws, but never deploy it or reuse it in production code.

## Custom agents

Two specialized agents live in `.github/agents/`:

- **@security-reviewer** reviews code for security vulnerabilities (OWASP, CWE, and language-specific patterns).
- **@compliance-checker** validates infrastructure as code, containers, and pipelines against CIS, NIST, and STIG guidance.

Reusable prompt files for common tasks are in `.github/prompts/`.

## Repository structure

```
Demos/            # Demo runbook for each lesson
Prompts/          # Security prompt library, organized by lesson
PPTs/             # Lesson slide PDFs
vulnerable-apps/  # Intentionally vulnerable apps (NodeGoat, WebGoat, TerraGoat, PyGoat)
.github/agents/   # Custom Copilot agents for security review and compliance
.github/prompts/  # Reusable prompt files
```

## Lessons

1. **Vulnerability detection with Copilot:** SQL injection, XSS, and custom scanners
2. **Implement security protocols:** authentication, encryption, and zero-trust infrastructure as code
3. **Automated security testing:** unit tests, fuzzing, SAST and DAST, and CI/CD gates
4. **Security code review, threat modeling, and auditing:** STRIDE, linters, and dependency review
5. **Compliance, incident response, and configuration management:** CIS, NIST, and STIG

## Technology used in the demos

- **Languages:** JavaScript and Node.js (primary), TypeScript, Python, Terraform, Bicep
- **Tools:** VS Code, GitHub Copilot, Copilot Chat, GHAS (CodeQL, Dependabot, secret scanning)
- **Security tooling:** Semgrep, OWASP ZAP, Azure CLI, ESLint security plugins

## Security content standards

When you help with a security task here, show the whole workflow:

1. Identify the vulnerable pattern and explain why it's exploitable.
2. Generate the secure alternative.
3. Write a security test that prevents the regression.
4. Show how the check runs in CI/CD.

Map findings to the OWASP Top 10 first, then add CWE identifiers and, for infrastructure, the relevant CIS Benchmark.

## Pitfalls to keep in mind

- Copilot's responses aren't deterministic, so verify every suggestion before you rely on it.
- Generic prompts produce generic, often incorrect, security advice. Include the architecture and the threat you care about.
- Copilot provides scaffolding. A human confirms that generated security code is correct.

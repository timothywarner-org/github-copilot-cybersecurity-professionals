# Security Policy

## Important Notice: Intentionally Vulnerable Applications

This repository is a **cybersecurity training course** that contains intentionally vulnerable
applications in the `/vulnerable-apps/` directory. These applications (NodeGoat, WebGoat,
TerraGoat, and PyGoat) are provided by OWASP and Bridgecrew for educational purposes and
contain **known, documented security vulnerabilities by design**.

**Do NOT deploy these applications to production environments, public-facing servers, or
any network accessible to untrusted users.** They are intended exclusively for local
development, isolated lab environments, and instructor-led training scenarios.

---

## Supported Versions

| Version | Supported          | Notes                                    |
|---------|--------------------|------------------------------------------|
| 1.x     | Yes                | Current course release, actively maintained |
| < 1.0   | No                 | Pre-release materials, no longer updated |

Security updates and patches are applied to the latest version only. If you are using
an older version of this course material, please update to the latest release.

---

## Reporting a Vulnerability

If you discover a security vulnerability in the **course infrastructure, repository
configuration, or supporting scripts** (not the intentionally vulnerable applications),
please report it responsibly.

### How to Report

1. **Do NOT open a public GitHub issue** for security vulnerabilities. Public disclosure
   before a fix is available puts all users at risk.

2. **Send an email to:** [tim@techtrainertim.com](mailto:tim@techtrainertim.com)

3. **Include the following information in your report:**
   - A clear description of the vulnerability
   - Steps to reproduce the issue
   - The affected file(s) or component(s)
   - The potential impact or severity (your assessment)
   - Any suggested remediation, if you have one

4. **Use a descriptive subject line** such as:
   `[SECURITY] Vulnerability in <component name>`

### What to Expect

| Milestone              | Target Timeframe |
|------------------------|------------------|
| Acknowledgment         | Within 48 hours  |
| Initial assessment     | Within 5 business days |
| Status update          | Within 10 business days |
| Resolution or mitigation | Depends on severity |

Critical vulnerabilities affecting active course participants will be prioritized.

---

## Responsible Disclosure Policy

We follow a coordinated disclosure process:

1. **Reporter** submits the vulnerability via email as described above.
2. **Maintainer** acknowledges receipt and begins assessment.
3. **Maintainer** works on a fix and communicates progress to the reporter.
4. **Maintainer** releases the fix and publishes a GitHub Security Advisory if warranted.
5. **Reporter** is credited in the advisory (unless they prefer to remain anonymous).

We ask that reporters:

- Allow reasonable time for the vulnerability to be addressed before any public disclosure.
- Make a good-faith effort to avoid accessing or modifying other users' data.
- Do not exploit the vulnerability beyond what is necessary to demonstrate the issue.

---

## Scope

### In Scope

- Repository configuration files (GitHub Actions workflows, CI/CD pipelines)
- Demo runbook scripts and automation code in `/Demos/`
- Prompt libraries and supporting tooling in `/Prompts/`
- Dependencies used by course infrastructure (not the vulnerable apps themselves)
- Any accidental exposure of secrets, credentials, or API keys

### Out of Scope

- Vulnerabilities in the intentionally vulnerable applications (`/vulnerable-apps/`).
  These are **expected** and are the subject of the course itself. If you find a
  vulnerability in NodeGoat, WebGoat, TerraGoat, or PyGoat that is not already
  documented by the upstream project, please report it to the respective OWASP or
  Bridgecrew project maintainers directly.
- Vulnerabilities in third-party tools referenced in the course (GitHub Copilot,
  CodeQL, OWASP ZAP, Checkov, Trivy, Semgrep). Report these to the respective
  vendors.

---

## Security Best Practices for Course Participants

Since this is a cybersecurity training course, we encourage participants to practice
good security hygiene while working through the material:

1. **Run vulnerable applications in isolated environments only** (Docker containers,
   virtual machines, or localhost with no external network exposure).
2. **Never commit real secrets** (API keys, passwords, tokens) to this or any repository.
   Use environment variables or secret management tools.
3. **Keep your tools updated.** Ensure Git, Docker, Node.js, Python, Java, and
   Terraform are at their latest stable versions.
4. **Review the `.gitignore` file** before committing changes to ensure sensitive
   files are excluded.
5. **Use GitHub's security features** such as Dependabot alerts, secret scanning,
   and code scanning as part of your learning.

---

## Acknowledgments

We appreciate the security research community and anyone who takes the time to
report vulnerabilities responsibly. Contributors who report valid security issues
will be acknowledged here (with their permission).

---

## Contact

- **Security Reports:** [tim@techtrainertim.com](mailto:tim@techtrainertim.com)
- **General Questions:** Open a [GitHub Issue](https://github.com/timothywarner-org/github-copilot-cybersecurity-professionals/issues)
- **Author Website:** [TechTrainerTim.com](https://techtrainertim.com)

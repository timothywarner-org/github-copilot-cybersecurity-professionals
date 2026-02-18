# Contributing to GitHub Copilot for Cybersecurity Professionals

Thank you for your interest in improving this course. This is a training repository
for a Pearson/Microsoft Press video course, so contributions are focused on accuracy,
quality, and compatibility rather than new feature development.

---

## Table of Contents

- [How You Can Help](#how-you-can-help)
- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Reporting Issues](#reporting-issues)
- [Submitting Pull Requests](#submitting-pull-requests)
- [Style Guidelines](#style-guidelines)
- [Development Setup](#development-setup)
- [Review Process](#review-process)

---

## How You Can Help

Contributions that improve the learning experience are welcome. The most valuable
contributions include:

- **Corrections** -- Fix typos, broken links, inaccurate instructions, or outdated
  command syntax in demo runbooks.
- **Compatibility notes** -- Document version-specific behavior, platform differences
  (macOS/Windows/Linux), or tool version incompatibilities you encounter.
- **Clarity improvements** -- Rewrite confusing instructions, add missing steps, or
  improve explanations in demo runbooks.
- **Tool updates** -- Flag when a referenced tool (CodeQL, OWASP ZAP, Checkov, etc.)
  has changed its CLI syntax or behavior.
- **Additional examples** -- Suggest new security scenarios, Copilot prompts, or
  vulnerability patterns that would strengthen the course.

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md).
By participating, you agree to uphold its standards. Please report unacceptable
behavior to [tim@techtrainertim.com](mailto:tim@techtrainertim.com).

---

## Getting Started

1. **Fork this repository** to your own GitHub account.

2. **Clone your fork locally:**

   ```bash
   git clone https://github.com/<your-username>/github-copilot-cybersecurity-professionals.git
   cd github-copilot-cybersecurity-professionals
   ```

3. **Create a feature branch** from `main`:

   ```bash
   git checkout -b fix/lesson-02-typo-correction
   ```

4. **Make your changes**, following the style guidelines below.

5. **Test your changes** by walking through the affected demo runbook steps.

6. **Push and open a pull request** against the `main` branch of this repository.

---

## Reporting Issues

Before opening a new issue, please search existing issues to avoid duplicates.

### Issue Types

Use the appropriate issue template when available:

- **Bug Report** -- A demo step does not work as described, a command fails, or
  tool output differs significantly from what the runbook shows.
- **Feature Request** -- A suggestion for a new demo scenario, tool integration,
  or course improvement.

### Writing Effective Issue Reports

A good issue report includes:

- The specific lesson and demo runbook step number where the problem occurs.
- Your operating system and version.
- The versions of relevant tools (Node.js, Python, Java, Terraform, Docker, VS Code).
- The exact error message or unexpected output.
- What you expected to happen instead.

---

## Submitting Pull Requests

### Branch Naming Convention

Use descriptive branch names with a category prefix:

| Prefix     | Purpose                          | Example                              |
|------------|----------------------------------|--------------------------------------|
| `fix/`     | Corrections and bug fixes        | `fix/lesson-03-zap-command-syntax`   |
| `docs/`    | Documentation improvements       | `docs/clarify-webgoat-setup-steps`   |
| `add/`     | New examples or scenarios        | `add/lesson-01-ssrf-detection-demo`  |
| `update/`  | Tool version or compatibility    | `update/terraform-1.7-syntax`        |

### Pull Request Requirements

Every pull request should:

1. **Reference an issue** if one exists (use `Fixes #123` or `Relates to #123`).
2. **Describe the change** clearly in the PR description.
3. **Limit scope** to a single logical change. Split unrelated changes into separate PRs.
4. **Test the affected steps** and confirm they work on your platform.
5. **Follow the style guidelines** documented below.

Use the pull request template provided in this repository.

---

## Style Guidelines

### Demo Runbooks

Demo runbooks in `/Demos/` are the core teaching artifacts. When editing them, follow
these conventions:

- **Use ATX-style headings** (`#`, `##`, `###`) with a blank line before and after.
- **Number procedural steps** using ordered lists (`1.`, `2.`, `3.`).
- **Fence all code blocks** with triple backticks and a language identifier:

  ````markdown
  ```bash
  npm install
  ```

  ```python
  import os
  ```
  ````

- **Include expected output** after commands when the output is important for
  verification.
- **Mark Copilot prompts** clearly so readers can distinguish between commands they
  type and prompts they give to Copilot.
- **Use plain language.** Write for an international audience. Avoid idioms, slang,
  and culturally specific references.
- **Do not use emojis** in runbook content.

### Markdown Standards

- Follow the repository's `.markdownlint.json` configuration.
- Use reference-style links for URLs that appear multiple times.
- Keep lines to a reasonable length (no hard wrap required, but avoid excessively
  long lines).
- Use blank lines to separate logical sections.

### Commit Messages

Follow conventional commit format:

```
<type>: <short description>

<optional body with details>
```

Types: `fix`, `docs`, `add`, `update`, `chore`, `refactor`

Examples:

```
fix: correct OWASP ZAP command syntax in Lesson 3 runbook

The --ajax flag was renamed to --ajaxSpider in ZAP 2.15. Updated the
demo runbook to use the current syntax.
```

```
docs: add Windows-specific setup notes for NodeGoat
```

---

## Development Setup

To work on this repository, you need the same tools described in the course README:

| Tool             | Minimum Version | Purpose                     |
|------------------|-----------------|-----------------------------|
| Git              | 2.x             | Version control             |
| Node.js          | 18.x            | NodeGoat                    |
| Python           | 3.9             | PyGoat                      |
| Java JDK         | 17              | WebGoat                     |
| Terraform        | 1.5             | TerraGoat                   |
| Docker Desktop   | Latest           | Container isolation         |
| VS Code          | Latest           | Editor with Copilot         |
| GitHub Copilot   | Active subscription | AI-assisted development |

You do not need all tools installed to contribute documentation fixes. Only install
what is needed to test the specific demo steps you are modifying.

---

## Review Process

1. **Automated checks** -- The PR will be checked for markdown lint compliance.
2. **Maintainer review** -- Tim Warner will review the PR for accuracy, style, and
   pedagogical fit.
3. **Feedback cycle** -- You may receive requests for changes. Address them in
   additional commits on the same branch.
4. **Merge** -- Once approved, the PR will be squash-merged into `main`.

Typical review turnaround is 5-10 business days. Course release deadlines may
affect response times.

---

## Questions?

- **General questions:** Open a [GitHub Issue](https://github.com/timothywarner-org/github-copilot-cybersecurity-professionals/issues)
- **Security concerns:** See [SECURITY.md](SECURITY.md)
- **Author contact:** [tim@techtrainertim.com](mailto:tim@techtrainertim.com)

Thank you for helping make this course better for everyone.

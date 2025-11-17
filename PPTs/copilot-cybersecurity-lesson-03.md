# Lesson 3: Automated Security Testing
**GitHub Copilot for Cybersecurity Specialists**

## Slide 1: Title
Lesson 3: Automated Security Testing

## Slide 2: Learning Objectives

• Generate AI-assisted security unit tests for critical functions  
• Create fuzz testing harnesses with Copilot for input validation  
• Automate DAST and SAST workflows for web and cloud applications  
• Build continuous security validation pipelines in CI/CD

## Slide 3: Generate Security Unit Tests with Copilot

• Security tests validate controls, not just functionality  
• Test the negative cases: attacks should fail  
• Use Copilot to generate edge cases from OWASP patterns  
• Build test libraries as reusable security assets

**Key Insight**: Your security test should PASS when the attack FAILS. You're validating that malicious input gets blocked, rejected, or sanitized.

## Slide 4: Demo - OAuth Authentication Test Suite

• Generate tests for token validation and expiration  
• Test authorization bypass attempts  
• Validate scope enforcement  
• Create reusable Copilot prompts for auth testing

**Focus**: Testing JWT signature verification, token expiration, refresh token rotation, and scope-based access controls.

## Slide 5: Fuzz Testing with AI Assistance

• Fuzzing generates unexpected inputs to find edge case vulnerabilities  
• Copilot creates domain-specific fuzzers for your application  
• Target injection points: forms, APIs, file uploads, search fields  
• Integrate fuzzing into automated test suites

**Attack Vectors**: SQL injection, XSS, command injection, buffer overflows, encoding bypasses.

## Slide 6: Demo - Input Validation Fuzzing

• Generate SQL injection test payloads with Copilot  
• Create XSS fuzzing harness for form inputs  
• Test file upload validation with malicious files  
• Analyze fuzzing results and prioritize fixes

**Payloads**: Union-based SQL attacks, boolean-based blind injection, XSS event handlers, double-extension files (.php.jpg).

## Slide 7: SAST/DAST Workflow Automation

• SAST finds vulnerabilities in source code without execution  
• DAST tests running applications like an attacker would  
• Integrate CodeQL for precise vulnerability detection  
• Combine OWASP ZAP with CI/CD for automated penetration testing

**Tool Stack**: CodeQL (SAST), OWASP ZAP (DAST), Semgrep (fast SAST), GitHub Advanced Security.

## Slide 8: Demo - CodeQL + Dependency Scanning

• Enable GitHub Advanced Security on your repository  
• Write custom CodeQL queries for business logic flaws  
• Configure dependency scanning for vulnerable packages  
• Create security gates in pull requests

**Security Gates**: Block PRs when critical/high vulnerabilities detected. Dependabot auto-creates PRs for vulnerable dependencies.

## Slide 9: CI/CD Security Gates

• Shift left: catch vulnerabilities before production  
• Automated security checks in every pipeline stage  
• Block deployments when critical vulnerabilities detected  
• Balance security with development velocity

**Pipeline Stages**: Unit tests on commit → SAST on PR → DAST on staging → Runtime monitoring in production.

## Slide 10: Demo - Pipeline Security Validation

• Create GitHub Actions workflow for security testing  
• Integrate multiple security tools in parallel  
• Configure security gates with appropriate thresholds  
• Generate security reports for compliance

**Workflow**: CodeQL + Semgrep + npm audit + OWASP ZAP running in parallel. Generate SARIF files for GitHub Security tab.

## Slide 11: Key Takeaways

• Automated security testing scales better than manual reviews  
• Security tests validate that attacks fail, not that features work  
• Integrate SAST, DAST, and fuzzing into your CI/CD pipeline  
• Build reusable security test libraries and Copilot prompts

**Bottom Line**: Systematic security testing = knowing your code is secure. Manual testing = hoping your code is secure.

## Slide 12: Next Steps

• Enable GitHub Advanced Security on one repository  
• Write security tests for your highest-risk code paths  
• Add security checks to your CI/CD pipeline  
• Build a library of reusable security prompts for Copilot

**Course Repository**: timw.info/copilot-security

---

## Tim's Teaching Notes

**Course Philosophy**: "We're showing security professionals how to channel Copilot toward security work. Every lesson should leave students feeling empowered with reusable patterns they can ship today."

**Key Themes**:
- Security tests check if code breaks safely when attacked
- Copilot generates edge cases from collective security knowledge
- Automation scales; manual testing doesn't
- Defense-in-depth through layered security checks

**Demo Flow**:
1. Show vulnerable code
2. Generate security tests with Copilot
3. Watch tests fail (proving vulnerability exists)
4. Apply fix
5. Watch tests pass (proving fix works)
6. Add to reusable test library

**Real-World Context**:
- Contoso reduced security test writing time 60%
- Fabrikam caught 90% of misconfigurations in PR review
- Adventure Works eliminated 90% of security defects reaching production
- Wide World Importers maintains 97% compliance with automated checks

**Pro Tips Applied**:
- Start with authentication/authorization (80% of critical vulns)
- Test with realistic attack payloads, not toy data
- Keep fuzzing payloads in version control
- Tune SAST/DAST tools for your codebase
- Start non-blocking, move to blocking gradually
- Make security check failures visible to whole team

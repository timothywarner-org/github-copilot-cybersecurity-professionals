<!-- Slide number: 1 -->

![A black background with grey text AI-generated content may be incorrect.](Picture5.jpg)
GitHub Copilot for Cybersecurity Specialists

![A blue and white rectangular sign with white text AI-generated content may be incorrect.](Picture6.jpg)

![](Picture9.jpg)

Tim Warner
Microsoft MVP
Microsoft Certified Trainer

### Notes

FRAMER: Welcome! I'm Tim Warner, and if you're writing code in 2025, GitHub Copilot is already in your workflow whether you planned for it or not. If you're responsible for security, that fact should concern you and excite you in equal measure. This course is about making Copilot your security tool instead of your security problem.

BULLETS:

This is Lesson 1 of our five-lesson journey into GitHub Copilot for Cybersecurity Specialists. Over the next 40 minutes, we're going to transform how you think about AI-assisted security work.

Here's what we're building toward: By the end of this course, you'll know how to use GitHub Copilot to detect vulnerabilities in your codebase, build hardened infrastructure with secure-by-default patterns, automate security testing, audit your code and dependencies, and prove compliance without drowning in manual work. Real patterns you can ship today.

All course resources, threat models, and Copilot prompts are staged at timw.info/copilot-security. You'll want that link handy throughout our time together.

PRO TIP: Bookmark the course repository now. The prompt templates alone will save you hours of experimentation. I've tested these patterns across dozens of real-world codebases, and they work.

<!-- Slide number: 2 -->

# Lesson 1: Vulnerability Detection with GitHub Copilot

Configure Copilot for security-focused development
Identify and remediate SQL injection vulnerabilities
Detect and prevent XSS attacks using AI assistance
Build custom vulnerability scanners for business logic flaws

### Notes

FRAMER: Attackers don't need to write SQL injection anymore - they just need to find it in your generated code. This lesson is about detection, remediation, and building repeatable workflows for continuous vulnerability discovery.

BULLETS:

• Configure Copilot for security-focused development - We're starting with the foundation. GitHub Copilot out of the box is optimized for velocity, not security. We'll reconfigure it to prioritize secure patterns, enable security-focused completions, and integrate with your existing security tools. Think of this as installing the safety equipment before we start working with power tools.

• Identify and remediate SQL injection vulnerabilities - SQL injection remains the number one application security risk according to OWASP 2024. We'll use Copilot Chat to scan existing code for injection patterns, then walk through step-by-step remediation using parameterized queries and prepared statements. You'll leave with reusable prompts for common injection scenarios.

• Detect and prevent XSS attacks using AI assistance - Cross-site scripting vulnerabilities hide in plain sight, especially in modern JavaScript frameworks. We'll demonstrate how Copilot can identify unsafe DOM manipulation, unsanitized user input, and missing Content Security Policy headers. Real talk: XSS is harder to spot than SQL injection because the attack surface is enormous. Copilot helps narrow that surface.

• Build custom vulnerability scanners for business logic flaws - Generic scanners miss the vulnerabilities that matter most to your organization: authorization bypasses in your custom API, race conditions in your payment flow, or privilege escalation in your admin panel. We'll use Copilot to build targeted scanners that understand your business logic. This is defense-in-depth that actually scales.

PRO TIP: The key to effective vulnerability detection with Copilot isn't asking "is this vulnerable?" It's providing enough context about your threat model that Copilot can reason about business logic flaws, not just generic OWASP patterns. We'll show you how to build that context.

<!-- Slide number: 3 -->

# Why This Matters: The Copilot Security Paradox

Copilot generates code at 55% faster velocity (GitHub, 2024)
AI-generated code contains vulnerabilities at similar rates to human code
Most organizations lack systematic AI code review processes
Security debt compounds at the speed of AI generation

### Notes

FRAMER: Here's the thing about AI-assisted development: We're generating technical debt faster than we can audit it. Let's talk about why systematic vulnerability detection isn't optional anymore.

BULLETS:

• Copilot generates code at 55% faster velocity (GitHub, 2024) - This is real data from GitHub's 2024 developer survey. Developers using Copilot ship features measurably faster. Fabrikam, a mid-size SaaS company, reported their feature velocity doubled after Copilot adoption. That's incredible for time-to-market. It's also incredible for attack surface expansion if you're not reviewing the generated code systematically.

• AI-generated code contains vulnerabilities at similar rates to human code - Multiple studies from Stanford and MIT show that Copilot-generated code has roughly the same vulnerability density as human-written code. The difference? Humans write 100 lines per day. Copilot helps you write 500. Same percentage, five times the volume. The math is brutal.

• Most organizations lack systematic AI code review processes - I work with dozens of enterprises every year. Most have sophisticated code review processes for human developers: mandatory security reviews, SAST integration, peer review requirements. Almost none have adapted those processes for AI-generated code. The result? Vulnerabilities slip through because reviewers assume "the AI wouldn't make that mistake."

• Security debt compounds at the speed of AI generation - Here's where it gets scary: If you're not detecting vulnerabilities as you generate code, you're building a backlog of security debt that grows faster than your security team can audit. Adventure Works learned this the hard way when a routine security audit found 47 SQL injection vulnerabilities in a six-month-old microservices implementation. All generated with Copilot assistance. All missed during code review.

PRO TIP: The security teams that win with Copilot are the ones who treat it like a force multiplier for security work, not just development work. That means using Copilot to detect the vulnerabilities that Copilot might help create. It's a beautiful kind of symmetry.

<!-- Slide number: 4 -->

# Demo 1: Configuring GitHub Copilot for Security

Install security-focused Copilot extensions
Configure workspace settings for secure defaults
Integrate with SAST/DAST tooling
Validate security-aware code completions

### Notes

FRAMER: Let's get hands-on. We're going to configure GitHub Copilot from the ground up for security-focused development. This is the foundation for everything else we build in this course.

BULLETS:

• Install security-focused Copilot extensions - Out of the box, Copilot is optimized for developer productivity. We need to layer on security-specific extensions that understand OWASP Top 10, CWE patterns, and secure coding standards. We'll install GitHub Advanced Security extensions, configure Copilot Chat for security queries, and enable vulnerability detection in real-time as you code.

• Configure workspace settings for secure defaults - VS Code workspace settings control how Copilot behaves at the project level. We'll configure settings.json to prefer secure patterns: parameterized queries over string concatenation, Content Security Policy headers by default, secure random number generation, and proper input validation. Think of this as teaching Copilot your organization's security preferences.

• Integrate with SAST/DAST tooling - Copilot doesn't replace your existing security tools - it augments them. We'll integrate Semgrep for static analysis, configure OWASP ZAP for dynamic testing, and show you how to use Copilot Chat to interpret security tool output. The beauty here is that Copilot can explain why a SAST tool flagged a particular line of code, not just that it's flagged.

• Validate security-aware code completions - How do you know if Copilot is generating secure code? We'll demonstrate testing methodology: inject known vulnerable patterns, observe Copilot's suggestions, and validate that security configurations are actually changing behavior. This is critical because Copilot learns from your codebase. If your codebase has vulnerabilities, Copilot will learn from them unless you intervene.

PRO TIP: Configuration drift is real. I recommend checking in your Copilot workspace settings to version control and treating them like infrastructure-as-code. When new team members clone your repository, they get security-aware Copilot by default. No manual configuration required.

<!-- Slide number: 5 -->

# Demo 2: SQL Injection Detection and Remediation

Identify vulnerable query patterns using Copilot Chat
Convert string concatenation to parameterized queries
Validate remediation with security unit tests
Build reusable detection prompts

### Notes

FRAMER: SQL injection is the classic web vulnerability, and it's still the most common critical finding in code audits. Let's use Copilot to find and fix it systematically.

BULLETS:

• Identify vulnerable query patterns using Copilot Chat - We'll start with a realistic Node.js API endpoint that concatenates user input directly into SQL queries. Classic vulnerability. We'll ask Copilot Chat: "Analyze this code for SQL injection vulnerabilities and explain the attack vector." Copilot will identify the vulnerable line, explain how an attacker could exploit it, and show example payloads. This is teaching moment gold.

• Convert string concatenation to parameterized queries - Once we've identified the vulnerability, we'll ask Copilot to refactor it. The prompt: "Refactor this code to use parameterized queries with prepared statements. Show both the vulnerable and secure versions side by side." Copilot will generate proper prepared statement syntax for your database driver, handle parameter binding correctly, and even update your error handling to account for parameterized query failures.

• Validate remediation with security unit tests - Fixing the vulnerability isn't enough - we need to prove it's fixed and prevent regression. We'll use Copilot to generate security-focused unit tests that attempt SQL injection against our API. If the test passes, the injection fails, which means our remediation worked. We'll include these tests in our CI pipeline so any future changes that reintroduce the vulnerability will fail the build.

• Build reusable detection prompts - Here's where Copilot becomes a force multiplier: We'll save our best prompts as reusable templates. "Scan this codebase for SQL injection using string concatenation" becomes a one-line audit tool. Contoso uses this pattern to audit 50+ microservices in minutes instead of weeks. The key is prompt refinement: Test, iterate, save the winners.

PRO TIP: When asking Copilot to detect vulnerabilities, always request both the vulnerable code and example exploit payloads. Understanding how the attack works makes you a better defender. I learned this from doing penetration testing for 15 years - you can't defend what you don't understand.

<!-- Slide number: 6 -->

# Demo 3: XSS Detection and Prevention

Scan for unsafe DOM manipulation and innerHTML usage
Detect missing output encoding in templates
Implement Content Security Policy headers
Validate XSS prevention with automated testing

### Notes

FRAMER: Cross-site scripting is harder to detect than SQL injection because the attack surface is everywhere user input touches the DOM. Let's use Copilot to narrow that surface systematically.

BULLETS:

• Scan for unsafe DOM manipulation and innerHTML usage - We'll analyze a React application with classic XSS vulnerabilities: unsanitized user input rendered directly to the DOM using innerHTML, dangerouslySetInnerHTML without DOMPurify, and missing escaping in template literals. We'll ask Copilot Chat to identify every instance where user-controlled data flows to dangerous sink functions. The beauty here is that Copilot understands data flow across multiple files, not just line-by-line analysis.

• Detect missing output encoding in templates - Modern frameworks like React and Vue provide automatic XSS protection, but only if you use them correctly. We'll demonstrate how Copilot can identify cases where developers bypass framework protections: double-curly-brace syntax that skips escaping, server-side rendering without sanitization, and unsafe attribute binding in Angular templates. Tailwind Traders found 23 instances of this pattern in a single codebase audit using Copilot-generated detection scripts.

• Implement Content Security Policy headers - CSP is your last line of defense against XSS. We'll use Copilot to generate appropriate CSP headers for our application: script-src directives that prevent inline JavaScript execution, object-src to block Flash/Java applets, and frame-ancestors to prevent clickjacking. Copilot will also help you understand CSP violations in your browser console and refactor code to comply with strict CSP policies.

• Validate XSS prevention with automated testing - We'll build a security test suite that attempts XSS injection against our application's input fields, URL parameters, and HTTP headers. These tests will include common payloads from PayloadsAllTheThings and custom payloads specific to our business logic. If the tests fail to execute JavaScript, our XSS prevention is working. We'll integrate this into our CI pipeline alongside our SQL injection tests.

PRO TIP: The most effective XSS prevention is defense-in-depth: Input validation on entry, output encoding at render time, and CSP as the last line of defense. Copilot can help you implement all three layers simultaneously. Don't skip layers because one layer feels "good enough" - attackers are creative.

<!-- Slide number: 7 -->

# Demo 4: Custom Vulnerability Scanners

Analyze business logic for authorization flaws
Build scanners for proprietary API patterns
Detect race conditions in critical workflows
Generate vulnerability reports with remediation steps

### Notes

FRAMER: Generic security scanners miss the vulnerabilities that matter most: authorization bypasses in your custom API, race conditions in your payment processing, or privilege escalation in your admin workflows. Let's build scanners that understand your business logic.

BULLETS:

• Analyze business logic for authorization flaws - We'll examine a multi-tenant SaaS application with a classic Insecure Direct Object Reference (IDOR) vulnerability: users can access other tenants' data by manipulating URL parameters. Generic scanners miss this because it requires understanding your authorization model. We'll use Copilot to write a scanner that understands "User A should never be able to access resources belonging to User B" and tests every API endpoint systematically.

• Build scanners for proprietary API patterns - Your organization has custom API patterns that generic tools don't understand: maybe you use JWT tokens with custom claims, or you have proprietary rate limiting logic, or you've built a custom API gateway with homegrown authentication. We'll use Copilot to generate scanners tailored to your patterns. The prompt: "Generate a scanner that validates our custom JWT token structure and tests for token manipulation vulnerabilities." Copilot will produce working code that understands your specific implementation.

• Detect race conditions in critical workflows - Race conditions are notoriously difficult to test because they're timing-dependent. We'll demonstrate how Copilot can help: analyzing critical workflows like payment processing, inventory updates, and coupon redemption for potential race conditions, then generating concurrent test harnesses that attempt to exploit those conditions. Wide World Importers found a race condition in their order processing that allowed customers to reuse discount codes by submitting multiple simultaneous requests. Copilot-generated tests caught it.

• Generate vulnerability reports with remediation steps - Once we've detected vulnerabilities, we need actionable reports that developers can actually use. We'll use Copilot to generate structured reports: vulnerability description, risk rating, affected code, example exploit, and step-by-step remediation guidance. These reports include code snippets for the fix, not just theoretical advice. This is the difference between "you have SQL injection" and "replace line 47 with this code."

PRO TIP: The secret to effective custom scanning is building up a library of prompts that understand your organization's specific threat model. Start with OWASP Top 10, then layer in your proprietary patterns. After 6-12 months, you'll have a scanner that's more effective than commercial tools for your specific codebase because it understands your business logic.

<!-- Slide number: 8 -->

# Key Takeaways: Vulnerability Detection

Configure Copilot for security-first development workflows
Systematic detection beats ad-hoc security reviews
Generic scanners miss business logic flaws
Build reusable prompts and integrate into CI/CD

### Notes

FRAMER: We've covered a lot of ground in this lesson. Let's solidify the key concepts before moving forward.

BULLETS:

• Configure Copilot for security-first development workflows - The single most important takeaway from this lesson: Copilot's behavior is configurable. Out of the box, it optimizes for velocity. With proper configuration, it can optimize for security instead. Install security-focused extensions, configure workspace settings for secure defaults, and integrate with your existing SAST/DAST tools. This foundation makes everything else in this course possible.

• Systematic detection beats ad-hoc security reviews - Manual code review doesn't scale when you're generating code at AI velocity. Systematic vulnerability detection using Copilot-generated scanners means every commit gets security validation without bottlenecking developers. Adventure Works reduced their security backlog by 73% after implementing systematic Copilot-assisted scanning in their CI pipeline.

• Generic scanners miss business logic flaws - SQL injection and XSS are important, but they're also well-understood by commercial scanners. The vulnerabilities that actually compromise organizations are business logic flaws: authorization bypasses, race conditions, and privilege escalation in custom code. Copilot helps you build scanners that understand your specific business logic and threat model. This is the difference between finding generic vulnerabilities and finding the vulnerabilities that matter.

• Build reusable prompts and integrate into CI/CD - Every effective security prompt you develop is infrastructure. Save it. Version control it. Integrate it into your CI/CD pipeline. Over time, you'll build a library of detection patterns that are specifically tuned to your codebase. This is your competitive advantage - commercial tools can't provide this level of customization.

PRO TIP: The teams that win with AI-assisted security are the ones who treat Copilot prompts like code: reviewed, tested, version controlled, and continuously improved. Start building your prompt library today. Six months from now, you'll have detection capabilities that would cost hundreds of thousands of dollars to build manually.

<!-- Slide number: 9 -->

# Next Steps

Practice: Run vulnerability detection on your codebase
Lesson 2: Implementing Security Protocols with Copilot
Resources: timw.info/copilot-security

### Notes

FRAMER: You now have the foundation for systematic vulnerability detection using GitHub Copilot. It's time to put these patterns into practice.

BULLETS:

• Practice: Run vulnerability detection on your codebase - Don't wait until Lesson 2 to start. Take the detection prompts we developed today and run them against your actual codebase. Start with SQL injection and XSS - these are high-impact and relatively straightforward to detect. Then move on to building custom scanners for your business logic. The sooner you start building your prompt library, the more effective you'll be when we layer on additional security protocols in the next lesson.

• Lesson 2: Implementing Security Protocols with Copilot - In our next lesson, we're moving from detection to implementation. We'll build secure authentication, authorization, encryption, and key management from first principles using Copilot to handle the boilerplate and enforce patterns. We'll design API gateway authentication, implement least privilege access controls, and layer in zero-trust network policies with infrastructure-as-code. This is defense-in-depth without the defense-in-chaos.

• Resources: timw.info/copilot-security - All course materials are available at the repository: detection prompts, vulnerable code samples, remediation templates, and security test suites. Everything we've demonstrated today is there. Use it. Adapt it to your environment. Share it with your team.

PRO TIP: Between lessons, I recommend experimenting with prompt engineering. The examples I've shown today are starting points, not final products. Refine them for your specific tech stack, threat model, and organizational context. The best security prompts are the ones you've customized through iteration and real-world testing.

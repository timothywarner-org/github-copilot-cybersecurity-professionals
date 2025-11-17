<!-- Slide number: 1 -->
# GitHub Copilot for Cybersecurity Specialists

Tim Warner
Microsoft MVP
Microsoft Certified Trainer

### Notes

FRAMER: Welcome to Lesson 2. In Lesson 1, we detected vulnerabilities with Copilot. Today we're implementing secure systems from first principles. We're not just writing secure code - we're using GitHub Copilot and GitHub Advanced Security to build authentication, encryption, API gateways, and zero-trust architectures that pass enterprise security reviews.

BULLETS:

The shift from Lesson 1 to Lesson 2 is moving from reactive to proactive security. Detection finds problems after they're written. Prevention eliminates problem classes before code hits production. GitHub Copilot excels at both, but today we're focusing on its ability to generate secure implementations that follow OWASP, NIST, and industry best practices automatically.

Over the next 40 minutes, we're implementing security protocols using GitHub Copilot's deep knowledge of security patterns combined with GitHub Advanced Security's policy enforcement. These aren't theoretical examples - this is production infrastructure that passes SOC 2 audits and enterprise security reviews.

All code samples, infrastructure-as-code templates, and Copilot prompts are at timw.info/copilot-security/lesson-02. You'll get working OAuth servers, encryption libraries, API gateway middleware, and Terraform templates you can deploy today.

PRO TIP: The most powerful thing about GitHub Copilot for security isn't that it writes secure code faster - it's that it knows security patterns you might not. OAuth 2.0 with PKCE, AES-256-GCM with proper IV generation, JWT validation with clock skew tolerance. These aren't obscure details - they're the difference between secure and broken implementations. Copilot gets them right automatically.

<!-- Slide number: 2 -->

# Lesson 2: Implement security protocols with GitHub Copilot

Build OAuth 2.0 authentication using Copilot's security pattern library
Implement encryption with Copilot-generated key management
Create API gateways with Copilot middleware and GHAS policy enforcement
Design zero-trust networks using Copilot infrastructure-as-code generation

### Notes

FRAMER: GitHub Copilot knows more about security implementation than most development teams combined. It's trained on millions of security implementations, OWASP guidelines, cryptographic libraries, and cloud security patterns. Our job is learning how to prompt it effectively and validate outputs with GitHub Advanced Security.

BULLETS:

• Build OAuth 2.0 authentication using Copilot's security pattern library - GitHub Copilot has seen thousands of OAuth implementations. It knows PKCE is required for public clients, it understands code challenge generation with SHA-256, and it implements proper redirect URI validation automatically. We're not memorizing RFC 6749 - we're prompting Copilot with our requirements and letting it generate specification-compliant OAuth servers. Then we'll use GitHub Advanced Security to validate that our implementation meets our security policy.

• Implement encryption with Copilot-generated key management - Copilot knows the difference between AES-CBC and AES-GCM. It knows PBKDF2 needs 600,000 iterations in 2024. It knows initialization vectors must be cryptographically random and unique per encryption operation. More importantly, it knows how to integrate with cloud key management services like Azure Key Vault. We provide business context, Copilot provides cryptographic correctness, and GHAS validates we're not committing keys to our repository.

• Create API gateways with Copilot middleware and GHAS policy enforcement - API gateways are security choke points. Every request validation happens here. Copilot generates JWT validation middleware that checks all required claims, rate limiting using Redis with sliding windows, and claim-based authorization that enforces least privilege. GitHub Advanced Security ensures our gateway code doesn't have vulnerabilities and our security policies are actually enforced in pull requests.

• Design zero-trust networks using Copilot infrastructure-as-code generation - Copilot generates Terraform for network segmentation, Istio configurations for service mesh with mutual TLS, and Azure Bicep templates for microsegmentation with network security groups. This is infrastructure-as-code for security - repeatable, auditable, and version-controlled. GHAS scans our IaC for misconfigurations before deployment.

PRO TIP: The workflow we're teaching today is prompt → generate → validate → deploy. GitHub Copilot generates secure implementations. GitHub Advanced Security validates they meet policy. You merge with confidence knowing both AI generation and automated security review happened. This is how security teams scale in 2025.

<!-- Slide number: 3 -->

# How GitHub Copilot understands security

Trained on cryptographic libraries and security frameworks
Understands OWASP Top 10, NIST guidelines, and compliance requirements
Generates secure-by-default code patterns automatically
Integrates with GHAS for policy enforcement and vulnerability detection

### Notes

FRAMER: Here's what makes GitHub Copilot different from generic code generation: It's trained specifically on security-focused codebases, cryptographic libraries, and compliance frameworks. It doesn't just autocomplete - it implements security patterns correctly.

BULLETS:

• Trained on cryptographic libraries and security frameworks - Copilot has seen implementations from Node.js crypto module, Python's cryptography library, Java's JCA, .NET's System.Security.Cryptography, and more. It understands that AES-GCM needs unique IVs, that PBKDF2 needs high iteration counts, and that JWT signatures should use RS256 not HS256. When you prompt for encryption, you get implementations that pass cryptographic review. Fabrikam stopped writing insecure encryption after adopting Copilot - the AI knew patterns their developers didn't.

• Understands OWASP Top 10, NIST guidelines, and compliance requirements - Copilot's training includes security standards documentation. It knows SQL injection prevention requires parameterized queries, XSS prevention requires output encoding, and authentication requires proper session management. More importantly, it understands compliance frameworks like PCI-DSS, HIPAA, and SOC 2. When you prompt for "PCI-DSS compliant encryption," Copilot generates code that meets those specific requirements. Wide World Importers reduced compliance audit findings by 73% after using Copilot for security implementations.

• Generates secure-by-default code patterns automatically - Traditional development: Write code, then retrofit security. With Copilot: Security is embedded from the first line. Request an API endpoint and Copilot includes input validation. Request authentication and Copilot includes rate limiting. Request data storage and Copilot includes encryption. Secure-by-default isn't a policy you enforce - it's the natural output of prompting an AI that understands security. Contoso reported 60% reduction in security backlog after Copilot adoption because vulnerabilities stopped being introduced.

• Integrates with GHAS for policy enforcement and vulnerability detection - GitHub Copilot generates code. GitHub Advanced Security validates it. GHAS runs CodeQL analysis that catches security vulnerabilities, secret scanning that prevents credential leaks, and dependency scanning that identifies vulnerable libraries. Together, they create a security validation loop: Copilot generates secure implementations, GHAS verifies they meet policy, pull requests merge with confidence. Adventure Works blocks an average of 23 security policy violations per week using this integration before code reaches production.

PRO TIP: The most powerful Copilot security feature isn't what it generates - it's what it prevents you from writing. When Copilot autocompletes a parameterized query, you don't write SQL injection. When it generates bcrypt password hashing, you don't store plaintext passwords. When it creates JWT validation with proper claims checking, you don't build broken authentication. Prevention at the code generation layer is force multiplication for security teams.

<!-- Slide number: 4 -->

# GitHub Advanced Security validates Copilot outputs

CodeQL scans Copilot-generated code for vulnerabilities
Secret scanning prevents credential leaks in repositories
Dependency scanning identifies vulnerable libraries in prompts
Security policies enforce standards before code merges

### Notes

FRAMER: GitHub Advanced Security is Copilot's validation layer. GHAS ensures that AI-generated code meets your security standards automatically. Let's explore how they work together.

BULLETS:

• CodeQL scans Copilot-generated code for vulnerabilities - CodeQL is GitHub's semantic code analysis engine. It understands code flow, data flow, and security patterns across 15+ languages. When Copilot generates authentication code, CodeQL validates that credentials aren't logged. When Copilot generates SQL queries, CodeQL verifies they're parameterized. When Copilot generates encryption, CodeQL checks for hardcoded keys. The integration is seamless: Write with Copilot, validate with CodeQL, merge with confidence. Tailwind Traders catches and fixes security issues in pull requests before they reach production using this workflow.

• Secret scanning prevents credential leaks in repositories - Copilot might suggest code that includes example API keys or test credentials. Secret scanning detects these patterns automatically and blocks commits. It scans for AWS keys, Azure secrets, GitHub tokens, private keys, and 200+ credential patterns. Real-time scanning means developers get immediate feedback: "This commit contains a detected secret - remove before push." Fabrikam prevented 47 credential leaks in six months using GHAS secret scanning integrated with their development workflow.

• Dependency scanning identifies vulnerable libraries in prompts - When Copilot suggests npm install jsonwebtoken or pip install cryptography, GHAS dependency scanning validates those packages don't have known vulnerabilities. It checks against GitHub Advisory Database, National Vulnerability Database, and security advisories from language ecosystems. If a vulnerable version is detected, you get an alert with remediation guidance before the dependency is merged. Adventure Works maintains zero critical-severity dependency vulnerabilities using automated scanning.

• Security policies enforce standards before code merges - GHAS lets you define security policies that block pull requests if violations are detected. Policy examples: All API endpoints must have rate limiting. All password storage must use bcrypt. All encryption must use approved algorithms. Copilot generates code, GHAS validates against policy, non-compliant code doesn't merge. This is automated security review that scales. Contoso enforces 15 security policies organization-wide using GHAS, blocking 200+ policy violations per month before they reach production.

PRO TIP: The workflow I recommend for security teams: Enable GitHub Advanced Security organization-wide. Configure security policies that match your standards. Then let developers use Copilot freely. GHAS catches violations automatically. This inverts the traditional model where security teams review after code is written. Now security validation happens during code generation. I've worked with teams that reduced security review cycle time from 2 weeks to 2 hours using this approach.

<!-- Slide number: 5 -->

# Using Copilot to implement secure authentication

Copilot generates OAuth 2.0 with PKCE from specification knowledge
Auto-completes JWT tokens with proper claims and secure signing
Suggests refresh token rotation patterns from security best practices
GHAS CodeQL validates authentication implementations

### Notes

FRAMER: Authentication is complex. OAuth 2.0 specs are hundreds of pages. JWT has subtle security requirements. Refresh token rotation has race conditions. GitHub Copilot knows all of this. Let's see how it generates secure authentication.

BULLETS:

• Copilot generates OAuth 2.0 with PKCE from specification knowledge - PKCE (Proof Key for Code Exchange) prevents authorization code interception attacks. It requires generating a code_verifier, hashing it to create code_challenge, validating them match during token exchange, and properly handling single-use authorization codes. That's complex implementation. With Copilot, you prompt: "Generate OAuth 2.0 authorization server with PKCE" and it produces specification-compliant code including SHA-256 code challenge generation, proper state parameter handling, and redirect URI validation. We'll demonstrate this live - you'll see 100+ lines of correct OAuth implementation generated in seconds.

• Auto-completes JWT tokens with proper claims and secure signing - JWT tokens have mandatory claims (iss, sub, aud, exp, iat) and security requirements (short expiration, RS256 signing, audience validation). Copilot knows these. Start typing "generate JWT token" and Copilot suggests implementations with all required claims, proper RS256 asymmetric signing using key pairs, short 15-minute expiration, and validation logic that checks all claims including clock skew tolerance. Fabrikam replaced their broken HS256 JWT implementation with Copilot-generated RS256 tokens and eliminated token forgery attacks.

• Suggests refresh token rotation patterns from security best practices - Refresh token rotation means: issue new refresh token with each use, revoke old tokens immediately, handle concurrent requests with Redis transactions, maintain revocation lists. Copilot generates this entire pattern including Redis integration, proper expiration handling, atomic token swap operations, and graceful failure modes. The key insight: Copilot's training includes security frameworks like OAuth RFCs and OWASP guidelines. It generates industry best practices automatically. We'll show live generation of refresh token rotation including all edge cases.

• GHAS CodeQL validates authentication implementations - After Copilot generates authentication code, GitHub Advanced Security runs CodeQL queries that validate: credentials aren't logged, tokens aren't exposed in URLs, session fixation is prevented, and timing attacks are mitigated. We'll demonstrate GHAS catching a deliberately introduced vulnerability in Copilot-generated code. This shows the validation layer: AI generates, GHAS verifies, humans review. Three-layer security review that scales.

PRO TIP: When prompting Copilot for authentication, be specific about your threat model. "Generate login endpoint" gets basic code. "Generate login endpoint with rate limiting, credential stuffing defense, and bcrypt password hashing" gets hardened implementation. Copilot's output quality scales with prompt specificity. In demonstrations today, you'll see detailed prompts that produce enterprise-grade authentication. Save these prompts - they're reusable across projects.

<!-- Slide number: 6 -->

# Using Copilot to implement encryption and key management

Copilot selects correct cipher modes and generates secure IVs
Integrates cloud key management services automatically
Implements key derivation with current iteration counts
GHAS secret scanning prevents key leaks in repositories

### Notes

FRAMER: Encryption has countless ways to fail. Wrong cipher mode, reused IVs, weak key derivation, hardcoded keys. GitHub Copilot prevents these failures by generating cryptographically correct implementations.

BULLETS:

• Copilot selects correct cipher modes and generates secure IVs - AES has multiple modes: CBC, GCM, CTR. Most developers don't know which to use when. GCM provides authentication, CBC doesn't. GCM needs 12-byte IVs, CBC needs 16-byte. IV must be unique per encryption operation. Copilot knows all this. Prompt "encrypt data with AES" and Copilot suggests AES-256-GCM with crypto.randomBytes IV generation, proper authentication tag handling, and IV prepending to ciphertext. Contoso replaced developer-written encryption that used hardcoded IVs with Copilot-generated encryption and eliminated a critical vulnerability found during penetration testing.

• Integrates cloud key management services automatically - Keys belong in Azure Key Vault or AWS KMS, not in code or environment variables. Copilot generates Key Vault integration including: authentication using DefaultAzureCredential, key fetching at runtime with caching, automatic refresh before expiration, and graceful fallback when Key Vault is unavailable. We'll demonstrate Copilot generating complete Key Vault integration in under 30 seconds. This is cloud-native key management that passes enterprise security review without manual implementation of credential chains and retry logic.

• Implements key derivation with current iteration counts - PBKDF2 transforms passwords into encryption keys. OWASP recommends 600,000 iterations in 2024 (up from 100,000 in previous years). Copilot knows current recommendations. Generate password-based encryption and Copilot suggests 600,000 iterations, SHA-512 hash function, 128-bit random salt generation, and proper salt storage. The AI stays current with security guidance - your generated code follows 2024 best practices automatically. Wide World Importers uses Copilot-generated PBKDF2 in production serving millions of users.

• GHAS secret scanning prevents key leaks in repositories - Even when Copilot generates secure encryption, developers might accidentally commit keys during testing. GHAS secret scanning catches this: detects encryption keys, API tokens, private keys, and credential patterns in commits. Blocks push with remediation guidance. We'll demonstrate secret scanning catching an intentionally committed key. This is the safety net: Copilot generates secure implementations, developers test with real credentials, GHAS prevents credential leaks. Tailwind Traders prevented 89 secret leaks in production using this workflow.

PRO TIP: When working with encryption, use Copilot Chat to understand why implementations work, not just what they do. Ask: "Why does AES-GCM need unique IVs?" Copilot explains the cryptographic reasoning. This builds your security knowledge while generating correct implementations. I recommend prompting for both implementation and explanation. The explanation helps you evaluate if Copilot's output matches your requirements. Understanding cryptography makes you better at validating AI-generated encryption code.

<!-- Slide number: 7 -->

# Using Copilot to secure API gateways

Generates complete middleware stacks with security controls
Implements rate limiting using distributed caching patterns
Creates claim-based authorization from security requirements
GHAS policies enforce gateway security standards

### Notes

FRAMER: API gateways are your security enforcement point. Every request flows through here. GitHub Copilot generates production-ready gateway middleware that would traditionally take weeks to implement and test.

BULLETS:

• Generates complete middleware stacks with security controls - API gateway security requires: JWT validation, rate limiting, input validation, CORS configuration, request size limits, and authorization checks. That's six separate middleware components working together. Copilot generates the entire stack from a single detailed prompt. We'll demonstrate prompting for "API gateway with JWT validation, rate limiting, and claim-based authorization" and watching Copilot produce Express middleware that implements all three with proper error handling, logging, and monitoring integration. Fabrikam's API gateway runs entirely on Copilot-generated middleware protecting 50+ backend microservices.

• Implements rate limiting using distributed caching patterns - Effective rate limiting requires: per-user limits, per-API-key limits, sliding window algorithms that prevent burst circumvention, Redis-backed counters for distributed enforcement, and exponential backoff for repeated violations. This is complex distributed systems implementation. Copilot generates it including Redis integration, atomic counter operations, proper key expiration, and handling Redis unavailability gracefully. The AI understands rate limiting algorithms from training on security frameworks. We'll show generated rate limiting code that handles all edge cases including clock skew and concurrent requests.

• Creates claim-based authorization from security requirements - Authorization at scale means: checking user roles from JWT claims, validating permissions from OAuth scopes, implementing role hierarchies where admin inherits lower privileges, and supporting fine-grained permissions like "read:orders vs write:orders". Copilot generates reusable middleware factory functions that implement these patterns. Prompt "generate authorization middleware that checks for admin or editor role" and Copilot produces middleware with role validation, proper 403 error responses, audit logging, and graceful handling of missing claims. Adventure Works centralized authorization in their API gateway using Copilot-generated middleware and reduced authorization bugs by 85%.

• GHAS policies enforce gateway security standards - GitHub Advanced Security lets you define policies like "All API endpoints must have rate limiting" or "All requests must be validated against schema". GHAS scans pull requests and blocks merges if policies aren't met. We'll demonstrate creating a security policy that requires JWT validation on all endpoints, then showing GHAS catching a pull request that adds an endpoint without authentication. This is automated security review at the gateway layer - the most critical security choke point in your architecture.

PRO TIP: API gateway security is where you get maximum leverage. One well-configured gateway protects dozens of backend services. Use Copilot to generate the gateway middleware, then reuse it across all services. Contoso uses a single Copilot-generated gateway package deployed to 73 services. When they need to update security controls, they update the gateway package and redeploy. That's security that scales. Start with the gateway - it's your highest-impact security investment.

<!-- Slide number: 8 -->

# Using Copilot to implement zero-trust architecture

Generates infrastructure-as-code for network segmentation
Creates service mesh configurations with mutual TLS
Produces compliance tests that validate security policies
GHAS scans IaC for misconfigurations before deployment

### Notes

FRAMER: Zero-trust architecture is "never trust, always verify." GitHub Copilot generates the infrastructure-as-code that implements zero-trust principles automatically. This is security architecture generated from natural language prompts.

BULLETS:

• Generates infrastructure-as-code for network segmentation - Zero-trust starts with network segmentation: separate subnets for web, application, and data tiers with network security groups that enforce least privilege communication. Traditionally, you'd write hundreds of lines of Terraform or Bicep manually. With Copilot, prompt "Generate Terraform for three-tier network with zero-trust segmentation" and watch it produce VNet configuration, subnet definitions, NSG rules that implement default-deny, and flow logs for audit. We'll demonstrate live Terraform generation including all security best practices. Wide World Importers deployed their entire production network using Copilot-generated Terraform. Zero manual Terraform writing required.

• Creates service mesh configurations with mutual TLS - Service mesh implements zero-trust at the service layer: every service authenticates to every other service using certificates. Istio configuration for this is complex YAML with authorization policies, certificate management, and telemetry integration. Copilot generates it automatically. Prompt "Generate Istio configuration with mTLS between all services" and Copilot produces complete service mesh configuration including automatic certificate provisioning, authorization policies that implement least privilege, and deny-by-default rules. Tailwind Traders implemented zero-trust service mesh using entirely Copilot-generated Istio configuration protecting 200+ microservices.

• Produces compliance tests that validate security policies - Zero-trust isn't set-and-forget. You need continuous validation that policies are actually enforced. Copilot generates automated compliance tests: verify cross-tier communication is blocked, validate service-to-service authentication is required, check that encryption in transit is enforced, ensure no default-allow rules exist. These tests run in CI/CD pipelines and fail deployments if zero-trust policies aren't met. We'll demonstrate Copilot generating a complete compliance test suite that validates network segmentation, service mesh authorization, and encryption enforcement. This is security-as-code meets testing-as-code.

• GHAS scans IaC for misconfigurations before deployment - Infrastructure-as-code can have security vulnerabilities just like application code. GHAS scans Terraform and Bicep for misconfigurations: overly permissive network rules, missing encryption, weak authentication requirements, and compliance violations. We'll demonstrate GHAS catching a network security group rule that allows internet access to the database tier - a critical misconfiguration. The workflow: Copilot generates IaC, GHAS validates security, humans review, Terraform deploys. Three-layer validation for infrastructure security.

PRO TIP: Zero-trust implementation is iterative. Don't try to implement everything at once. Start with network segmentation using Copilot-generated Terraform. Validate it works. Then add service mesh using Copilot-generated Istio configuration. Validate again. Then layer in compliance testing. This iterative approach prevents self-inflicted outages and builds confidence in your zero-trust implementation. I've worked with teams that implemented zero-trust in production with zero downtime using this phased approach. Copilot makes each phase faster because you're generating tested patterns, not inventing new ones.

<!-- Slide number: 9 -->

# Key takeaways: Security implementation with Copilot and GHAS

Copilot implements security patterns from cryptographic and compliance knowledge
GHAS validates outputs and enforces policies automatically
Prompt quality determines implementation quality - be specific
The workflow is generate → validate → deploy with confidence

### Notes

FRAMER: Let's solidify the core concepts from this lesson before moving to automated testing in Lesson 3.

BULLETS:

• Copilot implements security patterns from cryptographic and compliance knowledge - GitHub Copilot isn't generic code generation. It's trained on security-focused implementations including cryptographic libraries, OAuth specifications, OWASP guidelines, NIST standards, and cloud security frameworks. When you prompt for authentication, you get OAuth 2.0 with PKCE. When you prompt for encryption, you get AES-256-GCM with proper IVs. When you prompt for infrastructure, you get zero-trust segmentation. The AI knows security patterns that most developers don't. Adventure Works reduced security vulnerabilities by 67% after adopting Copilot because the AI prevented insecure patterns from being written in the first place.

• GHAS validates outputs and enforces policies automatically - GitHub Advanced Security is Copilot's validation layer. CodeQL scans for vulnerabilities in generated code. Secret scanning prevents credential leaks. Dependency scanning identifies vulnerable libraries. Security policies block merges if standards aren't met. This is automated security review that scales. Contoso uses GHAS to enforce 15 security policies organization-wide. Developers code freely with Copilot. GHAS catches violations automatically. Security team reviews edge cases only. Result: 80% reduction in security review bottlenecks.

• Prompt quality determines implementation quality - be specific - Vague prompts produce basic code. Specific prompts produce hardened implementations. Compare: "Generate login endpoint" versus "Generate login endpoint with bcrypt password hashing, rate limiting at 5 attempts per minute, Redis-backed session storage with 30-minute expiration, and audit logging of failed attempts." The second prompt produces enterprise-grade authentication. We demonstrated this pattern throughout today's lesson: detailed prompts that specify security requirements produce implementations that pass security review. Save your best prompts. They're reusable infrastructure.

• The workflow is generate → validate → deploy with confidence - Traditional development: Write code, security review finds vulnerabilities, rewrite, repeat. With Copilot plus GHAS: Prompt detailed requirements, Copilot generates secure implementation, GHAS validates against policy, humans review architecture, deploy with confidence. This workflow inverts the security review process. Security validation happens during code generation, not after. Fabrikam reduced security review cycle time from 2 weeks to 2 hours using this approach. The key: Automate what can be automated (Copilot generation + GHAS validation), focus humans on what can't (threat modeling and architecture review).

PRO TIP: The security protocols we implemented today - authentication, encryption, API gateways, zero-trust - are foundational but not sufficient. In Lesson 3, we're moving to automated security testing: fuzzing, DAST, SAST, and continuous security validation in CI/CD pipelines. Today we built secure systems. Next lesson we prove they're secure through comprehensive automated testing. That's the complete picture: generate with Copilot, validate with GHAS, test comprehensively, deploy with confidence.

<!-- Slide number: 10 -->

# Next steps

Practice: Use Copilot to implement authentication in your services
Lesson 3: Automated security testing with Copilot
Resources: timw.info/copilot-security

### Notes

FRAMER: You now understand how GitHub Copilot and GitHub Advanced Security work together to implement secure systems. Time to practice these patterns in your own environment.

BULLETS:

• Practice: Use Copilot to implement authentication in your services - Start with authentication because it has highest impact. Use the detailed prompts from today's lesson to generate OAuth 2.0 or JWT-based authentication. Deploy it in a non-production environment. Test it thoroughly. Then graduate to encryption, API gateways, and zero-trust infrastructure. The pattern is: learn through implementation, validate with GHAS, iterate based on feedback. Don't wait for perfect understanding - start implementing today.

• Lesson 3: Automated security testing with Copilot - In our next lesson, we're validating everything we built today. We'll generate AI-assisted security unit tests that verify authentication works correctly, create fuzz testing harnesses that discover edge cases in our API gateways, automate DAST and SAST workflows that scan our infrastructure-as-code for vulnerabilities, and build continuous security validation pipelines that prevent regressions. Today we built secure systems. Lesson 3 we prove they're secure through comprehensive automated testing.

• Resources: timw.info/copilot-security - All Copilot prompts, code samples, infrastructure-as-code templates, and GHAS policy examples from today's lesson are available in the course repository. These aren't toy examples - they're production patterns used by enterprises. Clone the repository, adapt the prompts to your tech stack, generate your own secure implementations.

PRO TIP: The biggest mindset shift required for Copilot adoption: Stop thinking of security implementation as something you do manually after writing code. Start thinking of it as something that happens automatically during code generation. Copilot generates secure patterns. GHAS validates they meet policy. Your job shifts from writing security code to validating security architecture. This is force multiplication for security teams. I've seen 3-person security teams support 200-person engineering organizations using this approach. The automation isn't replacing security expertise - it's amplifying it.

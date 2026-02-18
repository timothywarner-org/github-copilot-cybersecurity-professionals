---
description: "Validate infrastructure-as-code against CIS Benchmarks, NIST 800-53, and DISA STIG requirements"
---

Analyze the following infrastructure-as-code for compliance against these frameworks:

**CIS Benchmarks** - Check for:
- IAM and identity controls (least privilege, MFA, password policies)
- Logging and monitoring (CloudTrail/Activity Log, flow logs, retention)
- Networking (no unrestricted ingress on ports 22/3389, default SG restrictions)
- Encryption (at rest and in transit, CMK usage, TLS 1.2+)
- Storage (no public access, versioning, access logging)

**NIST SP 800-53 Rev 5** - Map findings to:
- AC (Access Control), AU (Audit), CM (Configuration Management)
- IA (Identification/Authentication), SC (System/Communications Protection)
- SI (System/Information Integrity)

**DISA STIG** - Classify findings as:
- CAT I (High) - Direct loss of CIA
- CAT II (Medium) - Potential loss with exploitation
- CAT III (Low) - Degrades security posture

For each finding provide:
1. The specific CIS control, NIST control family, or STIG rule ID
2. What the control requires
3. How the current code deviates
4. The remediated code
5. What evidence to retain for audit

End with a compliance summary table showing pass/fail counts per framework.

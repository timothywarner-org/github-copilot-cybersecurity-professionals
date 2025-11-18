# Lesson 05: All Compliance & Configuration Prompts

**Complete collection of 8 prompts for compliance automation**

---

## 1. Audit Terraform for CIS Compliance

```text
@workspace #file:storage.tf

Audit this Terraform configuration for security violations against:
1. CIS Azure Foundations Benchmark 1.5.0
2. Azure Security Baseline
3. NIST 800-53 controls for data protection

Identify specific issues with:
- Line numbers
- CIS benchmark control number violated
- NIST 800-53 control mapping
- Risk severity
- Remediation code snippet

Format as a security audit report.
```

**Use for**: IaC security review
**Customization**: Replace "Azure" with AWS, GCP; change CIS version

---

## 2. Generate CIS-Compliant IaC Modules

```text
Create a reusable Terraform module for Azure Storage Account that:

1. Meets ALL CIS Azure Foundations Benchmark 1.5.0 requirements for storage
2. Implements defense-in-depth:
   - Encryption in transit (TLS 1.2+)
   - Encryption at rest (Customer-Managed Keys)
   - Network isolation (Private Endpoints)
   - Access controls (Azure AD authentication, no SAS keys)
   - Logging and monitoring (Diagnostic settings to Log Analytics)
   - Versioning and soft delete enabled
3. Includes variables for configurability (environment, retention periods)
4. Adds validation blocks to enforce security requirements
5. Includes comprehensive tags for compliance tracking

Output should be production-ready with inline comments explaining each CIS control.
Save as modules/secure-storage/main.tf
```

**Use for**: Production IaC templates
**Customization**: Replace "Azure Storage" with any resource type; adjust standards

---

## 3. CIS Benchmark Validation Script

```text
Create a comprehensive PowerShell script that validates CIS Windows Server 2022 Benchmark controls:

Section 1: Account Policies (15 controls)
  - Password policies (complexity, length, age, history)
  - Account lockout policies

Section 2: Local Policies (25 controls)
  - Audit policies (logon events, account management, policy changes)
  - User rights assignments (interactive logon, service logon)
  - Security options (UAC, SMB signing, LDAP signing)

Section 3: Windows Firewall (12 controls)
  - Domain/Private/Public profile settings

Section 5: System Services (20 controls)
  - Disabled services that should not run

For each control:
1. Check actual system configuration
2. Compare against CIS benchmark requirement
3. Return Pass/Fail with evidence
4. Log failures with remediation guidance

Generate detailed HTML report with:
- Executive summary (compliance %)
- Control-by-control results
- Failed items with remediation steps
- Timestamp and system information

Save as scripts/Test-CISWindowsServer2022.ps1
```

**Use for**: Automated compliance validation
**Customization**: Replace "Windows Server 2022" with Linux, macOS; change CIS controls

---

## 4. CIS Compliance Scanning Automation

```text
Generate a GitHub Actions workflow for automated CIS scanning that:

1. Runs on schedule (weekly) and on-demand
2. Executes CIS validation script on multiple servers
3. Aggregates results across infrastructure
4. Generates compliance report
5. Fails if compliance rate drops below 90%
6. Posts summary to Slack/Teams
7. Creates tracking issues for failures

Save as .github/workflows/cis-compliance-scan.yml
```

**Use for**: Continuous compliance monitoring
**Customization**: Adjust schedule, thresholds, notification channels

---

## 5. PowerShell DSC for STIG

```text
Create a PowerShell Desired State Configuration that implements DoD STIG requirements for Windows Server 2022:

1. User Rights Assignments (50+ STIG controls)
2. Registry Security Settings (100+ STIG controls)
3. Service Configuration (40 services to disable per STIG)
4. Audit Policies (all 9 audit categories)
5. Windows Firewall (all 3 profiles)

Use the PowerSTIG module for baseline configuration.
Add custom configurations for:
- Guest account disabled
- Administrator account renamed
- Password policies
- Security options

Include monitoring to detect drift and auto-remediate.
Save as dsc/WindowsServer2022STIG.ps1
```

**Use for**: DoD STIG enforcement
**Customization**: Replace "Windows Server 2022" with other OS; adjust STIG version

---

## 6. STIG CI/CD Integration

```text
Generate a GitHub Actions workflow that:

1. Validates DSC configuration syntax (PSScriptAnalyzer)
2. Tests DSC in Docker container with Windows Server 2022
3. Generates STIG compliance report
4. Uploads report as artifact
5. Fails if CAT1 findings exist
6. Deploys DSC to Azure Automation for production servers

Save as .github/workflows/stig-compliance.yml
```

**Use for**: Automated STIG validation
**Customization**: Replace deployment target (Azure Automation, Ansible Tower, etc.)

---

## 7. Ransomware Incident Response Playbook

```text
Create a comprehensive incident response playbook for ransomware in an Azure environment:

1. Detection Phase:
   - Azure Sentinel KQL queries to detect ransomware indicators
   - File modification patterns (high volume file writes)
   - Lateral movement detection

2. Containment Phase:
   - PowerShell script to isolate compromised VMs
   - Disable affected user accounts
   - Block malicious IPs in NSGs
   - Snapshot all VMs for forensics

3. Eradication Phase:
   - Restore from Azure Backup
   - Scan restored systems with Microsoft Defender
   - Validate no persistence mechanisms remain

4. Recovery Phase:
   - Bring systems back online systematically
   - Reset all credentials
   - Update firewall rules

5. Lessons Learned:
   - Template for post-incident report
   - Metrics to track (time to detect, time to contain)

Include runnable PowerShell scripts for each phase.
Save as playbooks/ransomware-response.md with embedded scripts.
```

**Use for**: Incident response automation
**Customization**: Replace "Azure" with AWS, GCP, on-prem; adjust threat scenario

---

## 8. Automated Incident Containment

```text
Generate a PowerShell script for automated ransomware containment that:

1. Detects suspicious file encryption patterns
2. Immediately snapshots affected VMs
3. Isolates VMs from network (NSG deny-all rule)
4. Disables compromised user accounts
5. Sends alert to security team (email, Slack, PagerDuty)
6. Logs all actions for forensics
7. Provides rollback mechanism

Include error handling and safety checks (don't isolate domain controllers).
Use Azure PowerShell cmdlets for cloud operations.
Save as scripts/Invoke-RansomwareContainment.ps1
```

**Use for**: Automated incident response
**Customization**: Replace cloud provider; adjust detection logic

---

## Quick Reference: Compliance Frameworks

| Framework | Purpose | Prompts | Customization |
|-----------|---------|---------|---------------|
| **CIS Benchmarks** | Industry best practices | 1-4 | Azure, AWS, GCP, Linux, Windows |
| **DoD STIG** | Defense security | 5-6 | Windows, RHEL, Ubuntu |
| **NIST 800-53** | Federal compliance | 1-2 | All controls mapped |
| **PCI-DSS** | Payment card security | 1-2 | Storage, encryption, logging |
| **HIPAA** | Healthcare privacy | 1-2 | Encryption, access control |
| **SOC 2** | Service org controls | 7-8 | Incident response, monitoring |

---

## Compliance Control Mappings

### CIS → NIST 800-53

| CIS Control | NIST Control | Description |
|-------------|--------------|-------------|
| 3.1 (HTTPS required) | SC-8 | Transmission Confidentiality |
| 3.2 (Encryption at rest) | SC-28 | Protection at Rest |
| 5.1.5 (Logging) | AU-2 | Audit Events |
| 3.7 (Network isolation) | SC-7 | Boundary Protection |

### STIG → CWE

| STIG Finding | CWE | Description |
|--------------|-----|-------------|
| V-254230 (Guest account) | CWE-250 | Execution with Unnecessary Privileges |
| V-254298 (Legal notice) | CWE-1188 | Insecure Default Initialization |
| V-254448 (Audit policy) | CWE-778 | Insufficient Logging |

---

## Infrastructure Security Best Practices

### Terraform Security
```hcl
# ✅ Good: All security controls
resource "azurerm_storage_account" "secure" {
  enable_https_traffic_only = true
  min_tls_version          = "TLS1_2"
  allow_blob_public_access = false

  network_rules {
    default_action = "Deny"
  }
}

# ❌ Bad: Missing security
resource "azurerm_storage_account" "insecure" {
  # Defaults to HTTP allowed
  # No network restrictions
}
```

### PowerShell DSC
```powershell
# ✅ Good: Declarative, idempotent
Configuration SecureServer {
    WindowsFeature DisableSMB1 {
        Name   = 'FS-SMB1'
        Ensure = 'Absent'
    }
}

# ❌ Bad: Imperative script
Remove-WindowsFeature -Name 'FS-SMB1'
```

---

## Incident Response Phases

### 1. Preparation
- Documented playbooks
- Pre-configured automation
- Team training

### 2. Detection
- Security monitoring
- Anomaly detection
- Alerting

### 3. Containment
- Isolate affected systems
- Prevent spread
- Preserve evidence

### 4. Eradication
- Remove threat
- Patch vulnerabilities
- Verify clean

### 5. Recovery
- Restore systems
- Validate functionality
- Monitor for reinfection

### 6. Lessons Learned
- Post-incident review
- Process improvements
- Documentation updates

---

## Advanced Patterns

### Multi-Cloud Compliance

Prompt:
```text
Create a compliance validation framework that works across Azure, AWS, and GCP.
Use provider-agnostic checks where possible (encryption, logging, access control).
Generate unified compliance report mapping to CIS, NIST, and PCI-DSS.
```

### Compliance as Code Pipeline

Prompt:
```text
Generate a complete compliance pipeline that:
1. Validates IaC against security standards (terraform validate, checkov, tfsec)
2. Deploys infrastructure with compliance tags
3. Validates runtime configuration (CIS benchmarks)
4. Remediates drift automatically (DSC, Ansible)
5. Reports compliance status (dashboards, alerts)
```

---

## Success Metrics

Track these KPIs for compliance:

- **Compliance Rate**: % of controls passed
- **Mean Time to Remediate**: Days from detection to fix
- **Drift Rate**: % of systems that deviate from baseline
- **Auto-Remediation Rate**: % of issues fixed automatically
- **Audit Readiness**: Time to generate compliance report

---

[← Back to Lesson 05 Index](./README.md) | [Return to Prompts Home →](../README.md)

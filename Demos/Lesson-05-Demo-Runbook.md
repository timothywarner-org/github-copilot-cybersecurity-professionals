# Lesson 5: Compliance & Configuration Management

## Demo Runbook (15-20 minutes)

**Target Audience:** Professional developers implementing compliance automation and infrastructure security

**Demo Application:** Terragoat (deliberately vulnerable Terraform configurations)

**Tools Required:**

- VS Code with GitHub Copilot Enterprise
- GitHub Enterprise Cloud with Advanced Security
- Terraform 1.5+
- Azure CLI
- Python 3.10+
- PowerShell 7+ (for Windows Server demos)
- Docker

---

## Prerequisites (Complete before demo - 5 minutes)

### 1. Clone Terragoat

```bash
git clone https://github.com/bridgecrewio/terragoat.git
cd terragoat
```

### 2. Install IaC Security Scanning Tools

```bash
# Checkov for IaC security scanning
pip install checkov

# TFSec for Terraform security
brew install tfsec  # macOS
# or: curl -s https://raw.githubusercontent.com/aquasecurity/tfsec/master/scripts/install_linux.sh | bash

# Azure Policy for compliance
az extension add --name policy
```

### 3. Setup Azure Subscription (if demoing Azure)

```bash
az login
az account set --subscription "YOUR_SUBSCRIPTION_ID"
```

---

## Demo Part 1: Secure Infrastructure-as-Code Templates (6 minutes)

### Scenario

Terragoat contains deliberately vulnerable Terraform configurations. We'll harden them using Copilot to meet CIS Azure Foundations Benchmark.

### Step 1: Analyze Vulnerable Terraform

Open `terraform/azure/storage.tf` and use Copilot Chat:

**Copilot Prompt:**

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

**Expected Outcome:**

```text
TERRAFORM SECURITY AUDIT REPORT
================================

File: terraform/azure/storage.tf
Framework: CIS Azure Foundations Benchmark 1.5.0

FINDINGS:

[CRITICAL] Unencrypted Storage Account (Lines 15-25)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CIS Control: 3.1 - Ensure that 'Secure transfer required' is set to 'Enabled'
NIST 800-53: SC-8 (Transmission Confidentiality and Integrity)
Severity: CRITICAL
Risk: Data transmitted to/from storage without encryption

Current Configuration:
resource "azurerm_storage_account" "example" {
  name                     = "examplestorage"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "GRS"
  # Missing: enable_https_traffic_only
  # Missing: min_tls_version
}

Remediation:
resource "azurerm_storage_account" "example" {
  name                     = "examplestorage"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "GRS"

  # CIS 3.1: Enforce HTTPS
  enable_https_traffic_only = true

  # CIS 3.9: Require TLS 1.2 minimum
  min_tls_version = "TLS1_2"

  # CIS 3.7: Enable blob encryption
  blob_properties {
    versioning_enabled = true

    delete_retention_policy {
      days = 7
    }
  }

  # CIS 3.6: Disable public access
  allow_blob_public_access = false

  # NIST SC-8: Network security
  network_rules {
    default_action             = "Deny"
    ip_rules                   = []
    virtual_network_subnet_ids = [azurerm_subnet.example.id]
  }

  tags = {
    Environment = "Production"
    Compliance  = "CIS-Azure-1.5.0"
  }
}

[HIGH] Missing Encryption at Rest (Lines 15-25)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CIS Control: 3.2 - Ensure storage account encryption is enabled
NIST 800-53: SC-28 (Protection of Information at Rest)
Severity: HIGH

Add Customer-Managed Keys:
resource "azurerm_storage_account_customer_managed_key" "example" {
  storage_account_id = azurerm_storage_account.example.id
  key_vault_id       = azurerm_key_vault.example.id
  key_name           = azurerm_key_vault_key.example.name
}

[MEDIUM] No Diagnostic Logging (Lines 15-25)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CIS Control: 5.1.5 - Ensure diagnostic logging is enabled
NIST 800-53: AU-2 (Audit Events)
Severity: MEDIUM

Add Diagnostic Settings:
resource "azurerm_monitor_diagnostic_setting" "storage_logs" {
  name               = "storage-diagnostics"
  target_resource_id = azurerm_storage_account.example.id
  log_analytics_workspace_id = azurerm_log_analytics_workspace.example.id

  log {
    category = "StorageRead"
    enabled  = true

    retention_policy {
      enabled = true
      days    = 365
    }
  }

  metric {
    category = "Transaction"
    enabled  = true

    retention_policy {
      enabled = true
      days    = 365
    }
  }
}
```

### Step 2: Generate CIS-Compliant Terraform Module

**Copilot Prompt:**

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

**Expected Outcome:**

Production-ready Terraform module with all security controls:

```hcl
# CIS-Compliant Azure Storage Account Module
# Implements CIS Azure Foundations Benchmark 1.5.0
# NIST 800-53 Controls: SC-8, SC-28, AU-2, AC-3, IA-2

terraform {
  required_version = ">= 1.5.0"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

variable "storage_account_name" {
  description = "Name of the storage account (must be globally unique)"
  type        = string

  validation {
    condition     = length(var.storage_account_name) >= 3 && length(var.storage_account_name) <= 24
    error_message = "Storage account name must be between 3 and 24 characters."
  }

  validation {
    condition     = can(regex("^[a-z0-9]+$", var.storage_account_name))
    error_message = "Storage account name must contain only lowercase letters and numbers."
  }
}

variable "environment" {
  description = "Environment (dev, staging, prod)"
  type        = string

  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

variable "log_retention_days" {
  description = "Log retention period in days (CIS requires minimum 365 for production)"
  type        = number
  default     = 365

  validation {
    condition     = var.log_retention_days >= 90
    error_message = "Log retention must be at least 90 days per compliance requirements."
  }
}

# Main storage account resource with all CIS controls
resource "azurerm_storage_account" "this" {
  name                = var.storage_account_name
  resource_group_name = var.resource_group_name
  location            = var.location

  # Basic configuration
  account_tier             = "Standard"
  account_replication_type = var.environment == "prod" ? "GRS" : "LRS"
  account_kind             = "StorageV2"

  # CIS 3.1: Ensure 'Secure transfer required' is set to 'Enabled'
  # NIST SC-8: Transmission Confidentiality
  enable_https_traffic_only = true

  # CIS 3.9: Ensure storage account is using the latest version of TLS encryption
  # NIST SC-8: Cryptographic Protection
  min_tls_version = "TLS1_2"

  # CIS 3.6: Ensure that 'Public access level' is disabled for blob containers
  # NIST AC-3: Access Enforcement
  allow_blob_public_access = false

  # CIS 3.8: Ensure soft delete is enabled for Azure Storage
  # Data protection and recovery
  blob_properties {
    versioning_enabled = true

    delete_retention_policy {
      days = var.environment == "prod" ? 30 : 7
    }

    container_delete_retention_policy {
      days = var.environment == "prod" ? 30 : 7
    }

    # Prevent accidental overwrites
    last_access_time_enabled = true
  }

  # CIS 3.3: Ensure storage for critical data are encrypted with Customer Managed Key
  # NIST SC-28: Protection of Information at Rest
  # Note: Customer-managed key configured separately below
  identity {
    type = "SystemAssigned"
  }

  # CIS 3.12: Ensure that Azure File Shares are encrypted at rest
  # All Azure Storage is encrypted by default, but we enforce the setting
  azure_files_authentication {
    directory_type = "AADDS"  # Azure AD Domain Services authentication
  }

  # Network security - restrict access
  # CIS 3.7: Ensure default network access rule for Storage Accounts is set to deny
  # NIST SC-7: Boundary Protection
  network_rules {
    default_action             = "Deny"
    bypass                     = ["AzureServices"]
    ip_rules                   = var.allowed_ip_addresses
    virtual_network_subnet_ids = var.allowed_subnet_ids
  }

  # Compliance and security tags
  tags = merge(
    var.tags,
    {
      "CIS-Compliance"    = "Azure-1.5.0"
      "NIST-Controls"     = "SC-8,SC-28,AU-2,AC-3"
      "Environment"       = var.environment
      "ManagedBy"         = "Terraform"
      "SecurityReviewed"  = formatdate("YYYY-MM-DD", timestamp())
    }
  )

  lifecycle {
    prevent_destroy = true  # Prevent accidental deletion in production

    # Ensure security settings cannot be downgraded
    ignore_changes = []
  }
}

# CIS 3.2: Storage account encryption with Customer Managed Key
# NIST SC-28: Protection of Information at Rest
resource "azurerm_storage_account_customer_managed_key" "this" {
  count = var.use_customer_managed_key ? 1 : 0

  storage_account_id = azurerm_storage_account.this.id
  key_vault_id       = var.key_vault_id
  key_name           = var.key_vault_key_name
}

# CIS 3.10: Use private endpoints for storage
# NIST SC-7: Boundary Protection
resource "azurerm_private_endpoint" "blob" {
  count = var.enable_private_endpoint ? 1 : 0

  name                = "${var.storage_account_name}-blob-pe"
  resource_group_name = var.resource_group_name
  location            = var.location
  subnet_id           = var.private_endpoint_subnet_id

  private_service_connection {
    name                           = "${var.storage_account_name}-blob-psc"
    private_connection_resource_id = azurerm_storage_account.this.id
    subresource_names             = ["blob"]
    is_manual_connection          = false
  }

  tags = var.tags
}

# CIS 5.1.5: Ensure diagnostic logging is enabled for all services
# NIST AU-2: Audit Events
resource "azurerm_monitor_diagnostic_setting" "this" {
  name                       = "${var.storage_account_name}-diagnostics"
  target_resource_id         = azurerm_storage_account.this.id
  log_analytics_workspace_id = var.log_analytics_workspace_id

  # Blob service logs
  enabled_log {
    category = "StorageRead"

    retention_policy {
      enabled = true
      days    = var.log_retention_days
    }
  }

  enabled_log {
    category = "StorageWrite"

    retention_policy {
      enabled = true
      days    = var.log_retention_days
    }
  }

  enabled_log {
    category = "StorageDelete"

    retention_policy {
      enabled = true
      days    = var.log_retention_days
    }
  }

  # Metrics
  metric {
    category = "Transaction"
    enabled  = true

    retention_policy {
      enabled = true
      days    = var.log_retention_days
    }
  }

  metric {
    category = "Capacity"
    enabled  = true

    retention_policy {
      enabled = true
      days    = var.log_retention_days
    }
  }
}

# Outputs for module consumers
output "storage_account_id" {
  description = "The ID of the storage account"
  value       = azurerm_storage_account.this.id
}

output "primary_blob_endpoint" {
  description = "The endpoint URL for blob storage"
  value       = azurerm_storage_account.this.primary_blob_endpoint
}

output "storage_account_name" {
  description = "The name of the storage account"
  value       = azurerm_storage_account.this.name
}

output "cis_compliance_controls" {
  description = "CIS controls implemented by this module"
  value = [
    "CIS 3.1: Secure transfer required enabled",
    "CIS 3.2: Customer-managed encryption keys",
    "CIS 3.6: Public blob access disabled",
    "CIS 3.7: Default network access denied",
    "CIS 3.8: Soft delete enabled",
    "CIS 3.9: TLS 1.2 minimum enforced",
    "CIS 3.10: Private endpoints enabled",
    "CIS 5.1.5: Diagnostic logging enabled"
  ]
}
```

### Step 3: Validate with IaC Security Scanning

Run security scans on the improved configuration:

```bash
# Checkov scan
checkov -d modules/secure-storage/ --framework terraform

# TFSec scan
tfsec modules/secure-storage/
```

**Expected Outcome:**

```text
Check: CIS_AZURE_3_1: "Ensure 'Secure transfer required' is enabled" ... PASSED
Check: CIS_AZURE_3_2: "Ensure storage account encryption is enabled" ... PASSED
Check: CIS_AZURE_3_6: "Ensure default network access is denied" ... PASSED
Check: CIS_AZURE_3_7: "Ensure Network Security Groups flow log retention is >= 90 days" ... PASSED

Summary: 47 checks passed, 0 failed, 0 skipped
```

---

## Demo Part 2: Automated CIS Benchmark Compliance (5 minutes)

### Scenario

Automate CIS Windows Server 2022 benchmark validation using Copilot-generated PowerShell scripts.

### Step 1: Generate CIS Benchmark Validation Script

**Copilot Prompt:**

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

**Expected Outcome:**

```powershell
<#
.SYNOPSIS
    CIS Windows Server 2022 Benchmark Compliance Validation
.DESCRIPTION
    Validates system configuration against CIS Windows Server 2022 Benchmark Level 1
    Generates detailed compliance report with remediation guidance
.NOTES
    Author: Generated with GitHub Copilot
    Version: 1.0
    CIS Benchmark Version: 2.0.0
#>

[CmdletBinding()]
param(
    [string]$OutputPath = "$PSScriptRoot\CIS-Compliance-Report-$(Get-Date -Format 'yyyyMMdd-HHmmss').html",
    [switch]$RemediateFailures
)

# Result tracking
$results = @()
$passCount = 0
$failCount = 0

function Test-CISControl {
    [CmdletBinding()]
    param(
        [string]$ControlID,
        [string]$ControlName,
        [string]$Level,
        [scriptblock]$TestScript,
        [scriptblock]$RemediationScript,
        [string]$ExpectedValue,
        [string]$Rationale
    )

    Write-Verbose "Testing $ControlID - $ControlName"

    try {
        $actualValue = & $TestScript
        $isPassed = $actualValue -eq $ExpectedValue

        $result = [PSCustomObject]@{
            ControlID       = $ControlID
            ControlName     = $ControlName
            Level           = $Level
            Status          = if ($isPassed) { "PASS" } else { "FAIL" }
            ExpectedValue   = $ExpectedValue
            ActualValue     = $actualValue
            Rationale       = $Rationale
            Remediation     = if ($RemediationScript) { "Available" } else { "Manual" }
            Timestamp       = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
        }

        if ($isPassed) {
            $script:passCount++
            Write-Host "[PASS] $ControlID - $ControlName" -ForegroundColor Green
        } else {
            $script:failCount++
            Write-Host "[FAIL] $ControlID - $ControlName" -ForegroundColor Red
            Write-Host "       Expected: $ExpectedValue, Actual: $actualValue" -ForegroundColor Yellow

            if ($RemediateFailures -and $RemediationScript) {
                Write-Host "       Applying remediation..." -ForegroundColor Cyan
                & $RemediationScript
                $result.Status = "REMEDIATED"
            }
        }

        $script:results += $result
    } catch {
        Write-Error "Error testing $ControlID: $_"
        $script:results += [PSCustomObject]@{
            ControlID   = $ControlID
            ControlName = $ControlName
            Status      = "ERROR"
            ActualValue = $_.Exception.Message
        }
    }
}

# CIS Control 1.1.1: Enforce password history
Test-CISControl -ControlID "1.1.1" -ControlName "Enforce password history" -Level "L1" `
    -TestScript {
        (Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters" `
            -Name "PasswordHistorySize" -ErrorAction SilentlyContinue).PasswordHistorySize
    } `
    -ExpectedValue "24" `
    -Rationale "Password reuse increases risk of compromise" `
    -RemediationScript {
        Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters" `
            -Name "PasswordHistorySize" -Value 24 -Type DWord
    }

# CIS Control 1.1.2: Maximum password age
Test-CISControl -ControlID "1.1.2" -ControlName "Maximum password age" -Level "L1" `
    -TestScript {
        $secpol = secedit /export /cfg "$env:TEMP\secpol.cfg" /areas SECURITYPOLICY
        $maxAge = (Get-Content "$env:TEMP\secpol.cfg" | Select-String "MaximumPasswordAge").ToString().Split('=')[1].Trim()
        Remove-Item "$env:TEMP\secpol.cfg"
        return $maxAge
    } `
    -ExpectedValue "60" `
    -Rationale "Regular password rotation reduces compromise risk"

# CIS Control 2.2.1: Audit Credential Validation
Test-CISControl -ControlID "2.2.1" -ControlName "Audit Credential Validation" -Level "L1" `
    -TestScript {
        $auditPolicy = (auditpol /get /subcategory:"Credential Validation" | Select-String "Success and Failure").ToString()
        if ($auditPolicy -match "Success and Failure") { "Success and Failure" } else { "Not Configured" }
    } `
    -ExpectedValue "Success and Failure" `
    -Rationale "Credential validation auditing detects brute force attacks" `
    -RemediationScript {
        auditpol /set /subcategory:"Credential Validation" /success:enable /failure:enable
    }

# CIS Control 2.3.1.1: Accounts - Block Microsoft accounts
Test-CISControl -ControlID "2.3.1.1" -ControlName "Block Microsoft accounts" -Level "L1" `
    -TestScript {
        (Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" `
            -Name "NoConnectedUser" -ErrorAction SilentlyContinue).NoConnectedUser
    } `
    -ExpectedValue "3" `
    -Rationale "Microsoft accounts bypass organizational access controls" `
    -RemediationScript {
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" `
            -Name "NoConnectedUser" -Value 3 -Type DWord
    }

# CIS Control 2.3.11.3: Network security - Do not store LAN Manager hash
Test-CISControl -ControlID "2.3.11.3" -ControlName "Do not store LAN Manager hash value" -Level "L1" `
    -TestScript {
        (Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" `
            -Name "NoLMHash" -ErrorAction SilentlyContinue).NoLMHash
    } `
    -ExpectedValue "1" `
    -Rationale "LM hashes are cryptographically weak and easily cracked" `
    -RemediationScript {
        Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" `
            -Name "NoLMHash" -Value 1 -Type DWord
    }

# CIS Control 9.1.1: Windows Firewall - Domain Profile - State
Test-CISControl -ControlID "9.1.1" -ControlName "Windows Firewall Domain Profile Enabled" -Level "L1" `
    -TestScript {
        (Get-NetFirewallProfile -Name Domain).Enabled
    } `
    -ExpectedValue "True" `
    -Rationale "Firewall protects against network-based attacks" `
    -RemediationScript {
        Set-NetFirewallProfile -Name Domain -Enabled True
    }

# Generate HTML Report
$html = @"
<!DOCTYPE html>
<html>
<head>
    <title>CIS Windows Server 2022 Compliance Report</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        h1 { color: #333; }
        .summary { background-color: #f0f0f0; padding: 15px; border-radius: 5px; margin-bottom: 20px; }
        .pass { color: green; }
        .fail { color: red; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
        tr:nth-child(even) { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h1>CIS Windows Server 2022 Benchmark Compliance Report</h1>
    <div class="summary">
        <h2>Executive Summary</h2>
        <p><strong>Server:</strong> $env:COMPUTERNAME</p>
        <p><strong>Scan Date:</strong> $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')</p>
        <p><strong>Total Controls Tested:</strong> $($script:passCount + $script:failCount)</p>
        <p class="pass"><strong>Passed:</strong> $($script:passCount)</p>
        <p class="fail"><strong>Failed:</strong> $($script:failCount)</p>
        <p><strong>Compliance Rate:</strong> $(([Math]::Round(($script:passCount / ($script:passCount + $script:failCount)) * 100, 2)))%</p>
    </div>

    <h2>Detailed Results</h2>
    <table>
        <tr>
            <th>Control ID</th>
            <th>Control Name</th>
            <th>Status</th>
            <th>Expected</th>
            <th>Actual</th>
            <th>Rationale</th>
        </tr>
        $(
            $script:results | ForEach-Object {
                $statusClass = if ($_.Status -eq "PASS") { "pass" } else { "fail" }
                "<tr>
                    <td>$($_.ControlID)</td>
                    <td>$($_.ControlName)</td>
                    <td class='$statusClass'>$($_.Status)</td>
                    <td>$($_.ExpectedValue)</td>
                    <td>$($_.ActualValue)</td>
                    <td>$($_.Rationale)</td>
                </tr>"
            }
        )
    </table>
</body>
</html>
"@

$html | Out-File -FilePath $OutputPath -Encoding UTF8
Write-Host "`nCompliance report generated: $OutputPath" -ForegroundColor Cyan

# Return summary
[PSCustomObject]@{
    TotalControls  = $passCount + $failCount
    Passed         = $passCount
    Failed         = $failCount
    ComplianceRate = [Math]::Round(($passCount / ($passCount + $failCount)) * 100, 2)
    ReportPath     = $OutputPath
}
```

### Step 2: Run Compliance Scan

```powershell
# Run scan
.\Test-CISWindowsServer2022.ps1 -Verbose

# Run with auto-remediation (requires admin)
.\Test-CISWindowsServer2022.ps1 -RemediateFailures -Verbose
```

**Expected Output:**

```text
[PASS] 1.1.1 - Enforce password history
[FAIL] 1.1.2 - Maximum password age
       Expected: 60, Actual: 90
[PASS] 2.2.1 - Audit Credential Validation
[FAIL] 2.3.1.1 - Block Microsoft accounts
       Expected: 3, Actual: 0
       Applying remediation...
[PASS] 9.1.1 - Windows Firewall Domain Profile Enabled

Compliance report generated: C:\Scripts\CIS-Compliance-Report-20250317-143022.html

TotalControls  : 67
Passed         : 62
Failed         : 5
ComplianceRate : 92.54
ReportPath     : C:\Scripts\CIS-Compliance-Report-20250317-143022.html
```

---

## Demo Part 3: STIG Automation for Defense (5 minutes)

### Scenario

Implement DoD STIG compliance automation for Windows Server using PowerShell DSC.

### Step 1: Generate PowerShell DSC Configuration

**Copilot Prompt:**

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

**Expected Outcome:**

```powershell
#Requires -Modules PowerSTIG, ComputerManagementDsc, SecurityPolicyDsc, AuditPolicyDsc

Configuration WindowsServer2022STIG {
    param(
        [Parameter(Mandatory)]
        [string[]]$NodeName,

        [Parameter()]
        [string]$StigVersion = '2.6',

        [Parameter()]
        [ValidateSet('CAT1', 'CAT2', 'CAT3')]
        [string]$Severity = 'CAT2'
    )

    Import-DscResource -ModuleName PowerSTIG
    Import-DscResource -ModuleName SecurityPolicyDsc
    Import-DscResource -ModuleName AuditPolicyDsc
    Import-DscResource -ModuleName ComputerManagementDsc

    Node $NodeName {
        # Apply DoD STIG baseline
        WindowsServer BaseStigSettings {
            OsVersion   = '2022'
            StigVersion = $StigVersion
            Severity    = $Severity
            Exception   = @{
                # Allow exceptions for organizational requirements
                # V-254239: Organization requires 15-char passwords, not 14
                'V-254239' = @{
                    Identity = 'Passwords'
                    ValueData = 15
                }
            }
            OrgSettings = @{
                # Organization-specific STIG settings
                'V-254247' = @{
                    Identity = 'Legal Notice Caption'
                    ValueData = 'DoD Notice and Consent Banner'
                }
            }
        }

        # V-254230: Guest account must be disabled
        User DisableGuestAccount {
            UserName = 'Guest'
            Disabled = $true
            Ensure   = 'Present'
        }

        # V-254228: Built-in administrator account renamed
        User RenameAdministrator {
            UserName    = 'Administrator'
            Ensure      = 'Present'
            Description = 'Built-in account for administering the computer/domain'
            Disabled    = $false  # Required to be enabled per STIG
        }

        # V-254350: SMBv1 must be disabled
        WindowsFeature DisableSMB1 {
            Name   = 'FS-SMB1'
            Ensure = 'Absent'
        }

        # V-254362: Deny access to this computer from the network
        UserRightsAssignment DenyNetworkAccess {
            Policy   = 'Deny_access_to_this_computer_from_the_network'
            Identity = @('Guests', 'Local account')
        }

        # V-254448: Audit Credential Validation
        AuditPolicySubcategory CredentialValidation {
            Name      = 'Credential Validation'
            AuditFlag = 'SuccessAndFailure'
        }

        # V-254532: Windows Firewall Domain Profile must be enabled
        FirewallProfile DomainProfile {
            Name                  = 'Domain'
            Enabled               = 'True'
            DefaultInboundAction  = 'Block'
            DefaultOutboundAction = 'Allow'
            AllowUnicastResponseToMulticast = 'False'
            LogMaxSizeKilobytes   = 16384
            LogAllowed            = 'False'
            LogBlocked            = 'True'
            LogIgnored            = 'NotConfigured'
        }

        # V-254298: Legal notice must be displayed before logon
        Registry LegalNoticeCaption {
            Key       = 'HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System'
            ValueName = 'LegalNoticeCaption'
            ValueData = 'DoD Notice and Consent Banner'
            ValueType = 'String'
            Ensure    = 'Present'
        }

        Registry LegalNoticeText {
            Key       = 'HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System'
            ValueName = 'LegalNoticeText'
            ValueData = 'You are accessing a U.S. Government information system...'
            ValueType = 'String'
            Ensure    = 'Present'
        }

        # Configure LCM for continuous enforcement
        LocalConfigurationManager {
            ConfigurationMode  = 'ApplyAndAutoCorrect'  # Auto-remediate drift
            RebootNodeIfNeeded = $false
            RefreshMode        = 'Push'
            RefreshFrequencyMins = 30  # Check compliance every 30 minutes
            ConfigurationModeFrequencyMins = 15  # Reapply config every 15 minutes
        }
    }
}

# Compile DSC configuration
WindowsServer2022STIG -NodeName 'localhost' -OutputPath 'C:\DSC\STIG'

# Apply configuration
Start-DscConfiguration -Path 'C:\DSC\STIG' -Wait -Verbose -Force

# Monitor compliance
Get-DscConfiguration
Test-DscConfiguration
```

### Step 2: Integrate STIG Compliance into CI/CD

**Copilot Prompt:**

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

---

## Demo Part 4: Incident Response Playbook Automation (4 minutes)

### Scenario

Generate automated incident response playbooks using Copilot for common security incidents.

### Step 1: Generate Ransomware Response Playbook

**Copilot Prompt:**

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

**Expected Outcome:**

Comprehensive playbook with executable scripts for each phase:

```markdown
# Ransomware Incident Response Playbook

## Phase 1: Detection

### Azure Sentinel Detection Query
``kusto
// Detect ransomware file encryption patterns
SecurityEvent
| where EventID == 4663  // File access
| where AccessMask has_any ("0x2", "0x10")  // Write/Delete
| summarize FileModifications=count() by Account, Computer, bin(TimeGenerated, 5m)
| where FileModifications > 500  // Abnormal file modification rate
| project TimeGenerated, Computer, Account, FileModifications
| order by FileModifications desc
``

### PowerShell Detection Script
``powershell
# Detect-Ransomware.ps1
$suspiciousPatterns = @('.encrypted', '.locked', '.crypto', 'README_RANSOM')
$recentFiles = Get-ChildItem -Path C:\ -Recurse -ErrorAction SilentlyContinue `
    | Where-Object {
        $_.Extension -in $suspiciousPatterns -or
        $_.Name -like '*RANSOM*'
    } `
    | Where-Object { $_.LastWriteTime -gt (Get-Date).AddHours(-1) }

if ($recentFiles.Count -gt 10) {
    Write-Host "[ALERT] Possible ransomware detected: $($recentFiles.Count) suspicious files"
    # Trigger containment
}
``

## Phase 2: Containment (Execute Immediately)

``powershell
# Isolate-CompromisedVM.ps1
param(
    [Parameter(Mandatory)]
    [string]$ResourceGroupName,
    [string[]]$CompromisedVMNames
)

foreach ($vmName in $CompromisedVMNames) {
    # Snapshot VM for forensics
    $vm = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $vmName
    $snapshot = New-AzSnapshotConfig -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
        -Location $vm.Location -CreateOption Copy
    New-AzSnapshot -Snapshot $snapshot -SnapshotName "$vmName-incident-$(Get-Date -Format 'yyyyMMdd-HHmmss')" `
        -ResourceGroupName $ResourceGroupName

    # Isolate VM (block all traffic)
    $nsg = Get-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
        | Where-Object { $_.NetworkInterfaces.Id -contains $vm.NetworkProfile.NetworkInterfaces[0].Id }

    # Add deny-all rule
    $nsg | Add-AzNetworkSecurityRuleConfig -Name "INCIDENT-DenyAll" -Priority 100 `
        -Direction Inbound -Access Deny -Protocol '*' -SourcePortRange '*' `
        -DestinationPortRange '*' -SourceAddressPrefix '*' -DestinationAddressPrefix '*' `
        | Set-AzNetworkSecurityGroup

    Write-Host "[CONTAINED] VM $vmName isolated and snapshot created"
}
``
```

---

## Validation & Key Takeaways (1 minute)

### What We Demonstrated

1. **IaC Security**: Generated CIS-compliant Terraform modules with all security controls
2. **Compliance Automation**: Created PowerShell scripts validating CIS benchmarks
3. **STIG Enforcement**: Implemented DoD STIG with PowerShell DSC and auto-remediation
4. **Incident Response**: Generated executable IR playbooks for ransomware

### Compliance Automation Benefits

- **Continuous validation** replaces point-in-time audits
- **Auto-remediation** prevents configuration drift
- **Audit trails** via version control and automated reports
- **Scalability**: Same scripts validate 1 server or 1,000 servers

---

## Next Steps

1. Implement hardened IaC templates in your cloud environments
2. Automate CIS/NIST benchmark scanning on schedules
3. Deploy PowerShell DSC for STIG enforcement
4. Build organization-specific IR playbooks
5. Celebrate completing the course!

---

## Troubleshooting

**Issue**: Terraform validation failing

- **Solution**: Run `terraform validate` to check syntax
- Use `terraform fmt` to fix formatting
- Check provider version compatibility

**Issue**: PowerShell DSC not applying

- **Solution**: Ensure running as Administrator
- Check LCM status: `Get-DscLocalConfigurationManager`
- Review DSC logs: `Get-WinEvent -LogName 'Microsoft-Windows-Dsc/Operational'`

---

## Additional Resources

- Terragoat: <https://github.com/bridgecrewio/terragoat>
- CIS Benchmarks: <https://www.cisecurity.org/cis-benchmarks>
- DISA STIGs: <https://public.cyber.mil/stigs/>
- PowerSTIG: <https://github.com/Microsoft/PowerStig>
- Azure Policy: <https://learn.microsoft.com/azure/governance/policy/>

# PsExec Lateral Movement Investigation

## Objective

Investigate potential lateral movement activity using PsExec, PAExec, or similar remote administration tools detected by CrowdStrike Falcon.

---

## CrowdStrike Detection

### Detection Name

PsExec Lateral Movement

### MITRE ATT&CK

- T1021.002 - SMB/Windows Admin Shares
- T1569.002 - Service Execution

### Risk

CRITICAL

---

## Why This Matters

PsExec is a legitimate administration tool commonly abused by attackers after obtaining credentials.

It is frequently observed during:

- Ransomware Operations
- Domain Compromise
- Privilege Escalation
- Lateral Movement

---

## Common Tools

### PsExec

```cmd
psexec.exe \\TARGET cmd.exe
```

```cmd
psexec.exe \\SERVER powershell.exe
```

### PAExec

```cmd
paexec.exe \\TARGET cmd.exe
```

### Impacket

```cmd
psexec.py DOMAIN/user@host
```

---

## Initial Triage

### Detection Summary

Review:

- Hostname
- Username
- Detection Time
- Detection Severity
- Command Line
- Target Host

Determine:

- Source System
- Destination System
- Privilege Level Used

---

## Falcon Investigation Workflow

### 1. Detection Details

Collect:

- Detection ID
- Hostname
- Username
- SHA256
- Command Line
- Target Endpoint

Questions:

- Was PsExec executed successfully?
- Which credentials were used?
- What system was targeted?

---

### 2. Process Rollup2 Analysis

Review:

- Process Name
- Parent Process
- Grandparent Process
- SHA256
- MD5
- User Context
- Integrity Level

Common Processes:

- psexec.exe
- paexec.exe
- cmd.exe
- powershell.exe

---

### 3. Process Ancestry Analysis

Normal Example

```text
explorer.exe
 └── psexec.exe
```

Suspicious Example

```text
powershell.exe
 └── psexec.exe
```

Suspicious Example

```text
cmd.exe
 └── psexec.exe
      └── cmd.exe
```

Red Flags

- PowerShell launching PsExec
- Multiple remote executions
- Service creation followed by remote shell

---

### 4. Remote Service Investigation

PsExec creates a temporary service.

Review:

- Service Name
- Service Binary Path
- Service Start Time

Common Service Names:

```text
PSEXESVC
```

Investigate:

- Service Creation
- Service Start
- Service Removal

MITRE:

- T1569.002 Service Execution

---

### 5. SMB Activity Review

Review:

- SMB Connections
- Admin Share Access
- IPC$ Access
- C$ Access

Questions:

- Which systems were accessed?
- Was file transfer observed?
- Was malware copied?

---

### 6. Event Search Investigation

Review:

- ProcessRollup2
- SyntheticProcessRollup2
- DetectionSummaryEvent

Collect:

- Additional executions
- Related activity
- Timeline progression

---

### 7. Command Line Analysis

Review exact execution.

Examples:

```cmd
psexec.exe \\HOST cmd.exe
```

```cmd
psexec.exe \\HOST powershell.exe
```

```cmd
psexec.exe \\HOST -s cmd.exe
```

Questions:

- Was SYSTEM privilege used?
- Was PowerShell launched remotely?
- Was malware executed remotely?

---

### 8. Credential Investigation

Determine:

- Local Account
- Domain Account
- Service Account
- Domain Admin

Questions:

- Were privileged credentials used?
- Is account compromised?
- Was credential dumping observed previously?

---

### 9. Network Investigation

Review:

- Source IP
- Destination IP
- SMB Traffic
- Lateral Connections

Collect:

- Internal Hosts
- Remote Systems
- Additional Targets

---

### 10. Child Process Investigation

Determine what PsExec launched.

Common Examples:

- cmd.exe
- powershell.exe
- rundll32.exe
- mshta.exe
- certutil.exe

Red Flags:

- Remote PowerShell
- Encoded Commands
- LOLBin Activity
- Credential Dumping

---

### 11. Ransomware Indicators

Review:

- Shadow Copy Deletion
- Service Stopping
- Encryption Activity
- Backup Tampering

Common Commands:

```cmd
vssadmin delete shadows /all /quiet
```

```cmd
wmic shadowcopy delete
```

Questions:

- Is ransomware spreading laterally?
- Multiple hosts affected?

---

### 12. Active Directory Investigation

Review:

- Domain Admin Activity
- Kerberos Authentication
- Logon Events

Important Events:

- 4624
- 4625
- 4672
- 4768
- 4769
- 4776

Questions:

- Domain Admin Compromised?
- Multiple Host Logons?
- Suspicious Authentication Pattern?

---

## Scope Determination

### Host Scope

- Source Host
- Target Host
- Additional Systems

### User Scope

- Single User
- Multiple Users
- Privileged Accounts

### Network Scope

- Single Segment
- Enterprise Wide

---

## False Positive Validation

Validate:

### IT Administration

- SCCM
- Intune
- System Administration

### Authorized Activity

- Patch Deployment
- Server Management
- Remote Troubleshooting

Questions:

- Approved administrative activity?
- Change Request Available?
- Expected behavior?

---

## Containment Actions

### Immediate

- Isolate Source Host
- Isolate Target Host
- Disable Compromised Accounts
- Revoke Active Sessions

### Follow-Up

- Reset Credentials
- Hunt Enterprise-Wide
- Review AD Exposure
- Review Privileged Accounts

---

## Indicators of True Positive

- PsExec Execution
- Remote Service Creation
- SMB Admin Share Access
- Credential Dumping
- Remote PowerShell
- Lateral Movement
- Multiple Hosts Impacted

---

## Analyst Verdict

### True Positive

Unauthorized lateral movement confirmed.

### Suspicious

Further investigation required.

### False Positive

Authorized administrative activity validated.

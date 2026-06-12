# Business Email Compromise (BEC) Investigation

## Objective

Investigate suspected Business Email Compromise incidents and identify account takeover or fraudulent activity.

## Data Sources

- Microsoft Defender for Office 365
- Microsoft Entra ID
- Microsoft Sentinel

## Investigation Steps

### Step 1

Review suspicious email activity.

### Step 2

Check mailbox rules.

### Step 3

Review login history.

### Step 4

Check MFA activity.

### Step 5

Review sent items and forwarding rules.

## Containment

- Reset password
- Revoke sessions
- Remove mailbox rules
- Enable MFA

## MITRE ATT&CK

- T1078 Valid Accounts
- T1114 Email Collection

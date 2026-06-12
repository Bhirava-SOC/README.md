# Suspicious PowerShell Activity Investigation

## Objective

Investigate potentially malicious PowerShell execution.

## Data Sources

* CrowdStrike Falcon
* Defender XDR
* Microsoft Sentinel

## Investigation Steps

### Step 1

Review command line arguments.

### Step 2

Identify encoded commands.

### Step 3

Review parent-child process relationships.

### Step 4

Check network connections.

### Step 5

Identify persistence mechanisms.

## Containment

* Kill malicious process
* Isolate endpoint
* Block indicators

## MITRE ATT&CK

* T1059.001 PowerShell

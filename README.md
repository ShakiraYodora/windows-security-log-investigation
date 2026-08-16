# Windows Security Log Investigation

## Project Overview

This project demonstrates a hands-on SOC investigation using **Windows Event Viewer, Windows Security Logs, and PowerShell**.

I analyzed authentication activity and created a controlled security scenario involving a temporary local Windows account. I then used Windows Security logs to identify and correlate the events, reconstruct the activity timeline, determine whether the behavior should be considered suspicious, and document a final disposition.

## Investigation Scenario

A temporary local account named `SOC-Test` was created in a controlled lab environment.

The investigation identified a sequence of security events involving:

**Account Creation → Group Membership → Administrative Privileges → Account Deletion**

If this activity occurred unexpectedly on an organizational endpoint, the rapid creation of a new account followed by membership in the local Administrators group would require further investigation.

## Key Security Events

| Event ID | Activity                           | Analyst Interpretation            |
| -------- | ---------------------------------- | --------------------------------- |
| **4625** | Failed logon                       | Authentication failure identified |
| **4624** | Successful logon — Type 7          | Workstation successfully unlocked |
| **4720** | `SOC-Test` created                 | New local account activity        |
| **4732** | `SOC-Test` added to Users          | Standard local group membership   |
| **4732** | `SOC-Test` added to Administrators | Elevated privileges granted       |
| **4726** | `SOC-Test` deleted                 | Temporary account removed         |

## Investigation Findings

The authentication investigation identified a failed local logon followed approximately four seconds later by a successful workstation unlock. The activity was consistent with a user initially entering incorrect credentials and then successfully authenticating.

The account investigation identified more significant activity. The `SOC-Test` account was created and subsequently added to the local **Administrators** group.

Without known authorization, this sequence would initially be classified as:

**Suspicious — Investigate Further**

The events alone would not prove malicious activity. Additional investigation would be required to determine whether the account creation and privilege change were authorized.

## Final Disposition

**Benign — Authorized Lab Activity**

All activity was intentionally generated in a controlled environment for security-analysis training. No actual system compromise occurred.

The scenario demonstrated how multiple Windows Security events can be correlated to identify potentially suspicious account behavior.

## Evidence

Screenshots collected during the investigation are available in the [`evidence`](./evidence) folder.

Evidence includes:

* Event ID 4625 — Failed authentication
* Source address `127.0.0.1` — Localhost
* Event ID 4624 — Successful authentication
* Event ID 4720 — Local account creation
* Event ID 4732 — Account added to Users
* Event ID 4732 — Account added to Administrators
* Event ID 4726 — Account deletion

## Skills Demonstrated

* Windows Event Viewer
* Windows Security Log Analysis
* SOC Alert Triage
* Authentication Investigation
* Windows Event ID Analysis
* Logon Type Analysis
* Local Account Monitoring
* Privileged Group Monitoring
* Event Correlation
* Timeline Reconstruction
* Incident Classification
* PowerShell
* Security Documentation

## Full Investigation Report

The complete investigation, analyst findings, timeline, and supporting evidence are available in the **Project 3 SOC Analyst Windows Security Log Investigation PDF** included in this repository.

## Key Takeaway

This project reinforced that a single security event does not automatically indicate malicious activity. Effective SOC analysis requires reviewing context, correlating related events, reconstructing a timeline, and determining whether the observed behavior is expected, authorized, suspicious, or malicious.


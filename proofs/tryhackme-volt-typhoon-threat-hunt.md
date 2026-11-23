# TryHackMe — Volt Typhoon Threat Hunt

**Platform:** TryHackMe  
**Focus:** SOC Level 2 | Threat Hunting | Splunk | Windows telemetry  
**Tools:** Splunk, CyberChef  
**Status:** Lab investigation documented

## Overview

This investigation reconstructs activity associated with Volt Typhoon from ADSelfService Plus, WMIC, and PowerShell logs. The activity demonstrates living-off-the-land (LOTL) behaviour: the use of legitimate Windows utilities to perform discovery, credential access, persistence, staging, lateral movement, and defence evasion.

## Data sources

| Source | Splunk sourcetype | Purpose |
|---|---|---|
| ADSelfService Plus | `adss` | Password and account-management events |
| WMIC | `wmic` | WMI command execution and host discovery |
| PowerShell | `powershell` | PowerShell pipeline execution (Event ID 800) |

The lab data was searched in `index=main` on host `volthunter`.

## Investigation findings

### Account takeover and persistence

A completed password-change event for `dean-admin` was identified at **2024-03-24T11:10:22**. The ADSelfService Plus logs then showed an account update, followed by enrollment, unlock, password-change, security-question, and MFA actions for the newly introduced `voltyp-admin` account.

The password-change timestamp was used as the pivot point for subsequent searches.

### WMI discovery and remote activity

WMIC telemetry showed enumeration of operating-system and hardware details, network configuration, user accounts, shares, installed updates, processes, and event logs. Remote logical-disk queries against other servers indicated reconnaissance beyond the initially compromised host.

The logs also contained remote WMI process creation and a `netsh interface portproxy` command, consistent with internal pivoting.

### PowerShell and credential access

A PowerShell Event ID 800 record contained an encoded command launched with hidden-window and execution-policy-bypass options. Decoding the Base64 content in CyberChef revealed a download and execution sequence for Mimikatz, including use of an LSASS minidump.

### Collection and staging

PowerShell `Copy-Item` activity showed browser profile data and finance backup CSV files being copied into temporary directories. This is consistent with local staging before possible exfiltration.

### Archiving and defence evasion

A WMIC command invoked 7-Zip to create a password-protected, split archive from a file staged under the IIS web root. PowerShell logs also contained `Remove-Item` and `Remove-ItemProperty` commands, indicating file and registry-value cleanup.

## Attack timeline

| Time (UTC as represented in the lab logs) | Observed activity |
|---|---|
| 2024-03-24 11:10:22 | Successful password change for `dean-admin` |
| 2024-03-24 11:12–11:15 | Enrollment and security setup activity for `voltyp-admin` |
| 2024-03-26 21:15:18 | Registry query for virtualization-related values |
| 2024-03-26 21:53:41 | Encoded PowerShell command associated with Mimikatz |
| 2024-03-27 22:01–22:26 | Registry queries involving VNC and PuTTY settings |
| 2024-03-27 23:51–23:52 | Finance CSV files staged in a temporary directory |
| 2024-03-28 22:37 | Browser profiles copied to a temporary directory |
| 2024-03-29 13:45:02 | Failed password-change attempt for `dean-admin` |
| 2024-03-30 10:13:49 | Recursive registry enumeration |

Some WMIC archiving, port-proxy, and cleanup events were captured in the supplied evidence without a clearly visible timestamp; they are therefore not assigned invented times here.

## MITRE ATT&CK mapping

| Tactic | Technique | Lab evidence |
|---|---|---|
| Execution | T1047 — Windows Management Instrumentation | Remote WMIC process creation |
| Execution | T1059.001 — PowerShell | Encoded PowerShell execution |
| Persistence | T1136.002 — Create Account: Domain Account | Creation and setup of `voltyp-admin` |
| Defence Evasion | T1027 — Obfuscated Files or Information | Base64-encoded PowerShell |
| Defence Evasion | T1070 — Indicator Removal | File and registry-value removal |
| Credential Access | T1003.001 — LSASS Memory | Mimikatz and LSASS minidump activity |
| Credential Access | T1552.002 — Credentials in Registry | VNC and PuTTY registry queries |
| Credential Access | T1555.003 — Credentials from Web Browsers | Browser profile collection |
| Discovery | T1082, T1083, T1087, T1018, T1135 | System, file, account, remote-host, and share enumeration |
| Collection | T1074.001 — Local Data Staging | Data copied to temporary directories |
| Collection | T1560.001 — Archive via Utility | 7-Zip archive creation |
| Command and Control | T1090.001 — Internal Proxy | `netsh interface portproxy` |
| Persistence | T1505.003 — Web Shell | Web-shell names noted in the lab material; validate against primary lab evidence |

## Detection and mitigation ideas

- Alert on unusual `wmic /node:` and `wmic process call create` activity.
- Monitor changes to `netsh interface portproxy` configuration.
- Enable PowerShell logging and hunt for encoded commands, hidden windows, and execution-policy bypasses.
- Monitor suspicious LSASS access and use Credential Guard / LSA protection where appropriate.
- Alert on unexpected privileged-account creation and MFA/security-question enrollment.
- Hunt for bulk file copies into temporary directories and password-protected archive creation on servers.
- Review unusual Kerberos service-ticket activity and event-log access or clearing.
- Apply least privilege and network segmentation to limit lateral movement.

## Analyst takeaway

The investigation demonstrates why LOTL activity requires behavioural detection and correlation across multiple telemetry sources. A single account-management event provided a useful pivot, while WMIC and PowerShell logs exposed the broader sequence of discovery, credential access, data staging, remote execution, and cleanup.

## Evidence and scope notes

This portfolio summary is based on the supplied TryHackMe lab write-up. Sensitive-looking values such as passwords, archive secrets, and internal IP addresses have intentionally been omitted. Findings that were not directly timestamped or were supported only by secondary search results are identified as such rather than presented as independently verified facts.

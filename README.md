> 📌 **Portfolio Note:** This project is a standalone deep-dive module within the broader [SOC Attack Detection & Incident Response Suite](https://github.com/bermudd95/soc-attack-detection-labs).

# Windows Host Threat Hunting with Sysmon & EDR Telemetry

## Overview
This lab focuses on endpoint detection and response (EDR) analysis using granular Windows System Monitor (Sysmon) event logs. The goal is to detect evasive post-exploitation behavior—such as encoded PowerShell commands, process hollowing, and credential dumping—that evades standard antivirus solutions.

## Key Sysmon Event Identifiers Monitored

* **Event ID 1 (Process Creation):** Tracks command-line arguments, parent-child process relationships, and hash signatures.
* **Event ID 3 (Network Connection):** Correlates outbound C2 communication back to specific process IDs (PIDs).
* **Event ID 7 (Image Loaded):** Identifies suspicious DLL side-loading actions.
* **Event ID 10 (ProcessAccess):** Detects LSASS memory read requests associated with credential dumping tools (e.g., Mimikatz).

## Detection Scenarios & Analysis

### Scenario A: Encoded PowerShell Command Execution
* **Telemetry Signal:** `powershell.exe` spawned with `-EncodedCommand` or `-e` flags.
* **Analysis Methodology:** Extracted Base64 payload from Sysmon Event ID 1 telemetry, decoded payload via CyberChef, and identified outbound IP payload delivery.
* **Sigma / KQL Logic:**
```kql
process.pe.original_file_name : "PowerShell.EXE" AND process.command_line : ("*-EncodedCommand*" OR "*-e *")
```

### Scenario B: LSASS Memory Dumping

  * **Telemetry Signal:** Sysmon Event ID 10 where `TargetImage` ends in `lsass.exe` and `GrantedAccess` includes `0x1010` or `0x1F0FFF`.

  * **Mitigation:** Enforced LSA Protection (RunAsPPL) and configured Attack Surface Reduction (ASR) rules.

## Environment & Tech Stack

  * Endpoints: Windows 11 Enterprise (Sysmon v14 installed with SwiftOnSecurity config)

  * Log Ingestion: Windows Event Forwarding (WEF), EVTX Parsing, Elastic Security

  * Analysis Tools: PowerShell, CyberChef, Process Hacker / System Informer

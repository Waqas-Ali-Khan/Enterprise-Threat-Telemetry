# Threat Execution Report: Blue Locker Ransomware

## 1. Attack Overview
**Threat Actor:** Blue Locker
**Vector:** Malicious Executable (`BlueLocker.exe`)
**Objective:** High-speed, automated mass file encryption and denial of availability.

**Mechanics:** The attacker executed the ransomware binary from a world-writable directory (`C:\Users\Public\Downloads\`) using a hidden command prompt. The payload targets specific file extensions (`.xlsx`, `.pdf`), requests write-access, and generates encrypted copies appended with the `.blue` extension at a mathematically impossible speed for a human operator (5 milliseconds per file).

## 2. Raw Logs (The Millisecond Gap)
**Telemetry Source:** Windows Event Logs (ID 4663) & Sysmon (ID 11)

```json
{
  "Event": {
    "System": { "EventID": 4663, "TimeCreated": { "SystemTime": "2026-05-10T10:00:00.095Z" } },
    "EventData": {
      "ProcessName": "BlueLocker.exe",
      "ObjectName": "C:\\Data\\Budget.xlsx",
      "AccessMask": "0x2"
    }
  }
},
{
  "Event": {
    "System": { "EventID": 11, "TimeCreated": { "SystemTime": "2026-05-10T10:00:00.100Z" } },
    "EventData": {
      "TargetFilename": "C:\\Data\\Budget.xlsx.blue",
      "Image": "C:\\Users\\Public\\Downloads\\BlueLocker.exe"
    }
  }
}

# The Elite Differentiator: Multi-Stage Attack Correlation

## Operational Objective
Standard Tier-1 analysis often focuses on isolated alerts. This document proves the ability to track a complex, multi-stage intrusion across multiple telemetry sources (Network + Endpoint), mapping the attacker's progression from initial access to Command & Control (C2) beaconing.

## The Attack Narrative: "Operation Silent Hook"

### Stage 1: Initial Access (Phishing -> Delivery)
* **Time:** `08:15:02 UTC`
* **Telemetry:** Email Gateway / WAF
* **Event:** The user `CORP\Victim` receives a socially engineered email containing a link to a heavily obfuscated CDN. 
* **Log Evidence:** WAF logs show an HTTP GET request to a dynamic UUID `.js` file, bypassing static signature detection (as documented in the [Inferno Drainer Report](./InfernoDrainer_CryptoFraud_Telemetry.md)).

### Stage 2: Execution (LOLBin Activation)
* **Time:** `08:15:45 UTC`
* **Telemetry:** Windows Sysmon (Event ID 1 - Process Creation)
* **Event:** The downloaded payload executes a malicious `.lnk` file, which in turn spawns a hidden PowerShell process to run an encoded command in memory.
* **Log Evidence:** `powershell.exe -ExecutionPolicy Bypass -WindowStyle Hidden -EncodedCommand...` (as documented in the [SideWinder APT Report](./SideWinder_APT_Execution_Telemetry.md)).

### Stage 3: Defense Evasion & Impact (Ransomware Preparation)
* **Time:** `08:17:10 UTC`
* **Telemetry:** Windows Security Logs (Event ID 4663 - File System)
* **Event:** The spawned process begins requesting rapid `Write` access to critical network shares, preparing for a high-velocity encryption routine.
* **Log Evidence:** A 5-millisecond gap between file access and `.blue` file creation (as documented in the [Blue Locker Report](./BlueLocker_Ransomware_Telemetry.md)).

### Stage 4: Command & Control (C2 Beaconing)
* **Time:** `08:17:15 UTC`
* **Telemetry:** Wireshark PCAP / Firewall Logs
* **Event:** Before executing the final encryption trigger, the malware initiates an outbound TLS handshake to a known malicious IP (`45.33.xx.xx`) over Port 443 to exchange encryption keys.
* **Log Evidence:** Firewall telemetry shows sustained, rhythmic outbound connections (beaconing) of exactly 256 bytes every 60 seconds.

## Unified Response Strategy
If this sequence is detected by the SIEM:
1. **Sever the C2:** Block `45.33.xx.xx` at the perimeter firewall immediately to prevent key exchange.
2. **Quarantine the Host:** Isolate `CORP\Victim` via EDR.
3. **Hunt the IOCs:** Query the SIEM for any other hosts in the environment that have executed the specific Base64 encoded PowerShell string or resolved the malicious CDN domain.

# Elite Differentiator: Multi-Stage Attack Correlation Timeline

## 1. Attack Overview
**Objective:** Correlate disparate log sources into a single, unified narrative.
[cite_start]**The Flow:** Initial Phishing Email -> PowerShell Execution -> External C2 Communication -> Lateral Movement Attempt[cite: 86, 185].

## 2. Unified Master Timeline
| Stage | Source | Evidence | Analysis |
| :--- | :--- | :--- | :--- |
| **Infiltration** | Outlook/WAF | Phishing link clicked from `10.0.0.5` | [cite_start]Initial entry via social engineering[cite: 185]. |
| **Execution** | Sysmon (ID 1) | `powershell.exe -EncodedCommand...` | [cite_start]Malicious payload loaded into memory[cite: 185]. |
| **Persistence** | Sysmon (ID 11) | Malicious DLL dropped in `C:\Users\Public` | [cite_start]DLL Sideloading established[cite: 168]. |
| **C2 Callout** | Wireshark | HTTP GET to `cdn.malicious-drainer.io` | [cite_start]Beaconing to external Command & Control[cite: 185]. |
| **Lateral Movement** | Windows (ID 4624) | Remote logon attempt to `SRV-01` | [cite_start]Attacker attempting to pivot to server[cite: 185]. |

## 3. Detection & Response
* [cite_start]**Unified Logic:** Correlate `Event ID 1` (Execution) with immediate outbound traffic to a new domain.
* [cite_start]**Containment:** Isolate the source workstation and revoke all active directory tokens[cite: 199].

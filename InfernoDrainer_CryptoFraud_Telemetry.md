# Threat Execution Report: Inferno Drainer (Crypto Fraud)

## 1. Attack Overview
**Threat Actor:** Inferno Drainer
**Vector:** Malicious Web Injection / CDN Hosted Payload
**Objective:** Bypass static WAF blacklists to deliver a cryptocurrency wallet-draining script.

**Mechanics:** The attacker hosts the malicious JavaScript payload on a constantly rotating Content Delivery Network (CDN). To evade standard static blacklists, the attacker dynamically generates highly randomized, entropic 32-character UUIDs for the JavaScript file names. Because the file name changes for every single request, standard "blocklist" rules fail.

## 2. Raw Logs (Web Proxy / WAF Telemetry)
**Telemetry Source:** Apache Web Application Firewall (WAF) / Outbound Proxy

```json
{
  "timestamp": "2026-05-20T14:22:11.000Z",
  "src_ip": "192.168.1.105",
  "dest_host": "cdn.malicious-drainer.io",
  "http_method": "GET",
  "http_uri": "/assets/js/a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d.js",
  "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
  "waf_action": "allowed",
  "bypass_reason": "hash_not_in_database"
}

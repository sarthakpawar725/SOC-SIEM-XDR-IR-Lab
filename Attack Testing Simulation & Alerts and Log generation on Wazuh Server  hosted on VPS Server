
# Attack Simulation — Steps to Generate Alerts in Wazuh

## Preface
- Attacker: macOS Terminal
- Victim: Kali VM (has Wazuh Agent installed)
- SIEM: Wazuh on VPS
- IPs: replace <KALI_IP> and <MAC_IP> with your actual addresses

For each test:
1. Run the attack command(s) from your Mac terminal.
2. Wait ~10–60s for the agent to forward logs.
3. Open Wazuh Dashboard → Security Events and apply the suggested filter/query.
---

## Attack 1 — Simple failed SSH login (single attempt)
**Purpose:** Trigger authentication failure alert.

**From Mac:**
```bash
ssh wronguser@<KALI_IP>
# type an incorrect password once

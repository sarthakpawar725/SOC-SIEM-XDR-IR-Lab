# SOC- WAZUH SIEM-XDR-IR-Lab Hosted on Virtual Private Server
SOC / IR Simulation Project: Full-Cycle Threat Detection with Wazuh XDR/SIEM


# 🛡️ SOC Home Lab – Wazuh SIEM + Attack Simulation

A complete Security Operations Center (SOC) home lab built using **Wazuh SIEM**, a cloud VPS, and a **Kali Linux endpoint**.  
This lab simulates real-world cyber attacks and demonstrates log collection, alerting, detection engineering, and incident response.

---

## 🚀 Project Overview

This project demonstrates an end-to-end **SOC workflow**, including:

- Deployment of Wazuh SIEM on a cloud VPS
- Wazuh Agent installation on Kali Linux endpoint (XDR Endpoints)
- Host-based intrusion detection (HIDS)
- File Integrity Monitoring (FIM)
- SSH brute-force and authentication attack simulation
- Privilege escalation & system modification detection
- SIEM alert triage and investigation
- Basic incident response actions

This project replicates the responsibilities of an **L1/L2 SOC Analyst** in a realistic environment.

---

## 🧩 Components Used

| Component | Purpose |
|----------|---------|
| **Wazuh Manager** | Log processing & rule engine |
| **Wazuh Indexer** | Storage & search |
| **Wazuh Dashboard** | SIEM UI |
| **Kali Linux** | Monitored endpoint |
| **Wazuh Agent** | Log forwarding + FIM |
| **Mac Terminal** | Attack simulation |
| **DigitalOcean VPS** | Cloud SIEM host |

---

## 🛠️ Setup Summary

### 1️⃣ Installed Wazuh SIEM on VPS
- Ubuntu 22.04 LTS droplet deployed
- Installed Wazuh (Manager, Indexer, Dashboard)
- Secured access via HTTPS

### 2️⃣ Installed Wazuh Agent on Kali Linux
- Configured `/var/ossec/etc/ossec.conf`
- Connected agent to cloud SIEM
- Verified agent → manager communication
- Enabled File Integrity Monitoring (FIM)

### 3️⃣ Performed Attack Simulations (from Mac → Kali)
- SSH brute-force attempts  
- Failed login attempts  
- Successful SSH login  
- Privilege escalation (`sudo su`)  
- User account creation  
- File creation/modification  
- Permission changes  
- Service restarts  

### 4️⃣ Analyzed Alerts in Wazuh Dashboard
- Authentication failures
- Brute-force behavior
- Privilege escalation attempts
- New user creation
- File integrity alerts
- System modifications

### 5️⃣ Performed Basic Incident Response
- Reviewed `/var/log/auth.log`
- Identified attacker IP
- Blocked suspicious IP using UFW
- Removed unauthorized users
- Investigated FIM changes
- Documented findings

---

## 🔥 Example Alerts Observed

| Attack | Alert Type | Description |
|--------|------------|-------------|
| SSH brute-force | Authentication Failure | Multiple failed login attempts |
| `sudo su` | Privilege Escalation | User attempted elevated privileges |
| User creation | System Modification | New account created on endpoint |
| File edited | FIM Alert | Sensitive file modified |
| File permission change | FIM Alert | Executable permission added |

---

## 🔍 Incident Investigation Workflow

1. **Alert detected** in Wazuh Dashboard  
2. **Verify** source IP, user, and severity  
3. **Collect evidence** from logs:  

## 📘 Skills Demonstrated

- SIEM deployment & administration  
- Log analysis & alert triage  
- Host-based detection engineering  
- File Integrity Monitoring (FIM)  
- Authentication attack detection  
- Linux security monitoring  
- Basic incident response  
- Writing detection rules  
- SOC documentation & reporting  

---


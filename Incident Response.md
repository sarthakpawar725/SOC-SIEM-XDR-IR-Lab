## 🛡️ Incident Response for File Integrity Monitoring (FIM) in Wazuh

Wazuh’s File Integrity Monitoring (FIM) module records all file creation, modification, and deletion events occurring on monitored endpoints. When a suspicious or unexpected file change is detected, an incident response investigation is performed to understand the impact and determine whether the activity is malicious.

### 🔍 Identification
An incident begins when Wazuh raises a FIM alert, such as:
- **File added to the system**
- **File modified**
- **File deleted**
- **Integrity checksum changed**

The alert includes the monitored file path, event type, severity, and the agent where it occurred.

### 📁 Evidence Collection
Each FIM alert in Wazuh contains valuable evidence:
- **File path** (syscheck.path)  
- **Event type** (added / modified / deleted)  
- **Hash values before/after** (MD5, SHA1, SHA256)  
- **File size and permissions**  
- **User and group ownership**  
- **Real-time vs scheduled scan detection**  
- **Decoded raw log message**

Additional evidence can be taken from agent system logs, ossec.log, and audit events if process-level monitoring is enabled.

### 🧩 Analysis
The investigator reviews the file’s purpose, location, and context to determine whether the change is normal or suspicious. Examples:
- A new executable in a home directory may indicate malware delivery.  
- A modified configuration file may suggest tampering.  
- A deleted log file may indicate anti-forensic activity.  
- Unexpected hash changes could represent code injection or unauthorized edits.

Comparing the “before” and “after” states helps pinpoint what exactly changed.

### 🛑 Containment
If the activity is confirmed malicious or unapproved, appropriate containment actions are taken:
- Removing the suspicious file  
- Blocking the responsible user or process  
- Isolating the affected host  
- Using Wazuh Active Response to kill processes or quarantine files  

Real-time monitoring ensures rapid detection, reducing the attacker’s dwell time.

### 🔄 Recovery & Hardening
After containment, the system is restored to a trusted state.  
Hardening may include:
- Restricting write permissions  
- Adding directories to FIM monitoring  
- Enforcing stricter rules for sensitive paths  
- Applying patches or security controls  
- Creating custom Wazuh rules to prevent recurrence  

### 📘 Summary
File Integrity Monitoring in Wazuh is a critical part of incident response.  
It allows defenders to:
- Detect unauthorized file changes  
- Collect detailed forensic evidence  
- Understand attacker behavior  
- Contain threats quickly  
- Improve system integrity over time

This makes FIM one of the foundational components of host-based intrusion detection within the Wazuh platform.
## . Collect Additional Evidence (Commands)

### Check file ownership
ls -l /home/monu/thisistestfiletxt

### Check recent user commands
history | tail -n 20

### Check system logs
sudo tail -n 50 /var/log/syslog

### Check process activity (auditd)
sudo ausearch -f fim_test.txt


##  Determine Root Cause (Questions, no commands)

- Was the file expected?
- Who created or modified it?
- Why did the hash change?
- Is the file executable?
- Is it in a sensitive directory?
- Did this happen during off-hours?


##  Contain the Incident (Commands)

### Delete suspicious file
sudo rm /home/monu/fim_test.txt

### Kill malicious process
sudo pkill -f suspicious_binary

### Disable suspicious user
sudo usermod -L baduser

### Block suspicious external IP
sudo iptables -A INPUT -s <IP> -j DROP


##  Recover and Harden (Commands)

### Update and patch system
sudo apt update && sudo apt upgrade -y

### Example custom Wazuh rule (add to local_rules.xml)
<rule id="600101" level="10">
  <match>thisistestfile.txt.txt</match>
  <description>Unauthorized file created</description>
  <group>syscheck,</group>
</rule>


## 🟩 8. Finalize the Incident (Checklist)

- What file changed  
- When it happened  
- Who triggered it  
- What the file contained  
- Why it was suspicious  
- What containment steps were taken  
- How the system was hardened afterward  

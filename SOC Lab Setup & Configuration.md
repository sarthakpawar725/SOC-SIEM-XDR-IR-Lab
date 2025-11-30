
# 🛠️ SOC Lab Setup & Configuration

This document provides a detailed walkthrough of how I built my complete SOC home lab using:

- Wazuh SIEM (Hosted on VPS)
- Kali Linux VM (Monitored Endpoint)
- macOS (Attacker Machine)
- Wazuh Agent (Installed on Kali)

This lab simulates a real Security Operations Center environment and provides hands-on experience in monitoring, detecting, and investigating security events.

---

# 🔷 1. VPS Setup (Wazuh Server)

### ✔ Step 1: Create VPS on DigitalOcean
- Droplet created: **Ubuntu 22.04 LTS**
- Size used: **4GB RAM / 2 vCPU** (recommended for Wazuh)
- Authentication: **Password login**
- Firewall rule: Allowed ports `22`, `443`, `1514`, `1515`



---

### ✔ Step 2: Connect to VPS via SSH (from Mac)

ssh root@<VPS_IP>

Update and Install wazuh
apt update && apt upgrade -y
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
bash wazuh-install.sh -a
The installer automatically sets up:

Wazuh Manager
Wazuh Indexer
Wazuh Dashboard

### ✔ Step 3: Access Wazuh Dashboard

Open browser (Mac):

https://<VPS_IP>

Login:

Username: admin
Password: (from installation output

🔷 2. Kali Linux Setup (Victim Endpoint)

✔ Step 1: Add Wazuh Agent Repository
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | \
sudo gpg --dearmour -o /usr/share/keyrings/wazuh.gpg

echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] \
https://packages.wazuh.com/4.x/apt/ stable main" | \
sudo tee /etc/apt/sources.list.d/wazuh.list


✔ Step 2: Install Wazuh Agent
sudo apt update
sudo apt install wazuh-agent -y

Screenshot:

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/85fb9cd9-abe0-4d9f-878d-49298a48fe6d" />


sudo nano /var/ossec/etc/ossec.conf

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/267ba84b-ee43-45f5-b854-7743de30193e" />


Replace:

<address>MANAGER_IP</address>

With:

<address><YOUR_VPS_IP></address>

Restart agent:
sudo systemctl restart wazuh-agent ( Important to render on VPS )


✔ Step 4: Approve Agent in Wazuh Dashboard
Wazuh Dashboard → Agents → Pending → Accept


🔷 3. macOS Setup (Attacker Machine)

Your Mac is used to simulate attacks.

✔ Step 1: Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

✔ Step 2: Install Nmap
brew install nmap

✔ Step 3: Attack Kali VM Using:
- Failed SSH logins
- Brute-force loops
- Normal login
- System modifications inside Kali
- Many more attacks ( Will discuss Further )

  ✔ Step 4: Check if Wazuh agents are installed and running on kali machine

Screenshot: 

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/e18d5506-b2d5-4d38-a8b5-54783751d936" />




🔷 4. Enable File Integrity Monitoring (FIM)

✔ Step 1: Edit Wazuh Agent Config
sudo nano /var/ossec/etc/ossec.conf

Add inside <syscheck>:
<directories check_all="yes">/etc</directories>
<directories check_all="yes">/var/log</directories>
<directories check_all="yes">/home</directories>
<directories check_all="yes">/root</directories>

Restart agent:
sudo systemctl restart wazuh-agent



🔷 5. Basic Attack Simulations

Here are attacks performed from macOS → Kali:

🔥 SSH Brute-Force
ssh wrong@<KALI_IP>

🔥 Brute-Force Test
for i in {1..10}; do ssh root@<KALI_IP>; done

🔥 Privilege Escalation (inside Kali)
sudo su.

🔥 User Creation
sudo useradd hacker & 

🔥 File Modification (FIM Test)
echo "test123" | sudo tee -a /etc/testfile.txt

🔥 Many More Intresting Attacks and Testing


🔷 6. Our Wazuh Server and Dashboard are ready!

Screenshot:

<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/0ce4f07d-ba3a-4ebe-86f4-96fd55d0b0eb" />

📌 Summary

This lab successfully demonstrates:

- SIEM installation & configuration
- Endpoint monitoring
- Attack simulation
- Linux security fundamentals
- SOC workflows

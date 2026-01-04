🛡️ Suricata IDS Setup & Rule Testing Lab
📌 Overview

This repository documents a hands-on lab where I installed, configured, and validated Suricata IDS on a Linux system.
The goal of this lab is to understand Suricata’s architecture, rule management, traffic inspection, and alert generation using both standard and custom rules.

This setup mirrors a real SOC IDS deployment workflow: installation → configuration → rule updates → validation → monitoring.
🧱 Environment

    OS: Ubuntu / Kali Linux

    Suricata Version: 7.x (OISF Stable)

    Capture Mode: AF-PACKET

    Interface: enp0s3

    Logging Format: fast.log + eve.json

🛠️ Installation
Add Official OISF Repository

sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update

Install Suricata

sudo apt-get install suricata -y

⚙️ Service Management (Optional)

sudo systemctl enable suricata.service
sudo systemctl status suricata.service
sudo systemctl stop suricata.service

Stopping the service during configuration is recommended.
📂 Configuration Files

    Main config: /etc/suricata/suricata.yaml

    Default rules: /var/lib/suricata/rules/

    Custom rules: /etc/suricata/rules/local.rules

🔧 suricata.yaml Configuration
1️⃣ Define HOME_NET

vars:
  address-groups:
    HOME_NET: "192.168.2.0/24"

2️⃣ Configure AF-PACKET Capture

af-packet:
  - interface: enp0s3

3️⃣ Enable Community ID (Flow Correlation)

community-id:
  enabled: true

4️⃣ Rule Paths

default-rule-path: /var/lib/suricata/rules

rule-files:
  - suricata.rules
  - /etc/suricata/rules/local.rules

🔄 Updating Rule Sets
Update Default Rules

sudo suricata-update

Rules are downloaded and merged into:

/var/lib/suricata/rules/suricata.rules

List Available Rule Sources

sudo suricata-update list-sources

Enable Additional Source (Example)

sudo suricata-update enable-source win-malware
sudo suricata-update

✅ Configuration Testing

sudo suricata -T -c /etc/suricata/suricata.yaml -v

This validates:

    YAML syntax

    Rule loading

    Interface configuration

▶️ Start Suricata

sudo systemctl start suricata.service
sudo systemctl status suricata.service

📑 Log Monitoring

Logs are stored in:

/var/log/suricata/

    fast.log → human-readable alerts

    eve.json → structured JSON events (SIEM-ready)

Test Alert Generation

curl http://testmynids.org/uid/index.html

View Alerts

sudo cat /var/log/suricata/fast.log

✍️ Custom Rule Creation
Create Local Rule File

sudo vim /etc/suricata/rules/local.rules

Example ICMP Rule

alert icmp any any -> $HOME_NET any (msg:"ICMP Ping"; sid:1; rev:1;)

Reload & Test

sudo suricata -T -c /etc/suricata/suricata.yaml -v
sudo systemctl start suricata.service

Trigger with:

ping <target-ip>

📊 JSON Log Analysis with jq
Install jq

sudo apt-get install jq

Live Alert View

tail -f /var/log/suricata/eve.json | jq 'select(.event_type=="alert")'

This simulates real SOC alert monitoring.
🎯 Key Takeaways

    Deployed Suricata using official OISF sources

    Configured AF-PACKET traffic capture

    Managed rule sets and external feeds

    Created and validated custom IDS rules

    Verified detection using real traffic

    Monitored alerts in both classic and JSON formats

🚀 Next Improvements

    Forward eve.json to Elastic SIEM

    Add MITRE ATT&CK mapping

    Write detection rules for Nmap scans

    Reduce false positives via rule tuning

📎 Author

Nouaman
Cybersecurity & SOC Enthusiast
Hands-on defensive security labs focused on detection engineering and monitoring

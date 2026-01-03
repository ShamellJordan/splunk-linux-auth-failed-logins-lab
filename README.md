# splunk-linux-auth-failed-logins-lab
Homelab project: Detecting Linux authentication failures using Splunk SIEM
# Detecting Failed Authentication Attempts in Ubuntu Using Splunk Enterprise (Blue Team Lab)

## Overview
This project demonstrates end-to-end log monitoring and detection of failed authentication attempts in a Linux environment using Splunk Enterprise. I built an Ubuntu 22.04 virtual machine inside VirtualBox on macOS, configured Splunk to ingest system authentication logs, intentionally generated failed `sudo` attempts, and created searches to identify these events. This lab replicates real blue-team workflows used in Security Operations Centers (SOCs): deploying SIEM tools, ingesting logs, generating security-relevant events, and writing detection queries.

## Environment and Tools
- Host OS: macOS  
- Virtualization Platform: VirtualBox  
- Guest OS: Ubuntu 22.04  
- SIEM Platform: Splunk Enterprise  
- Primary Log Source: /var/log/auth.log  

## Lab Steps Performed

### Step 1 — Built Ubuntu 22.04 VM in VirtualBox
I created a new virtual machine in VirtualBox, allocated CPU/RAM/storage, attached the Ubuntu 22.04 ISO, installed the OS, applied updates, and verified the system was operational with working networking.

### Step 2 — Installed Splunk Enterprise inside the VM
I downloaded the Splunk Enterprise .deb package, installed it via dpkg, accepted the license, enabled Splunk to run, and accessed the Splunk Web UI via http://localhost:8000, creating the admin credentials.

### Step 3 — Configured Splunk to Ingest auth.log
Inside Splunk Web:
- Add Data → Monitor  
- Files & Directories  
- Added /var/log/auth.log  
- Indexed to “main”  

This connected Linux authentication logs as a data source for security monitoring.

### Step 4 — Generated Failed Authentication Attempts
To intentionally create authentication failure events, I repeatedly ran:

sudo ls

and entered incorrect passwords. These failed sudo attempts were written directly to /var/log/auth.log.

### Step 5 — Detected Events in Splunk
Inside Splunk Search & Reporting, I ran queries including:

index=main "incorrect password"
index=main "Failed password"

Splunk successfully detected and displayed the failed login attempts and sudo authentication failures, including timestamps, users, and event contents.

## Why This Matters for Blue Team / SOC Roles
Authentication logs reveal critical security behaviors such as password guessing, brute-force attacks, privilege escalation attempts, insider misuse, and compromised accounts. Blue team analysts and SOC analysts are expected to:
- ingest system logs into SIEM platforms  
- build detections and queries  
- validate alert results  
- distinguish false positives from true incidents  

This lab demonstrates practical experience with:
- Splunk configuration and indexing
- Linux log collection
- authentication event analysis
- creating detection logic based on log patterns

The ability to configure, search, and interpret authentication logs is a core SOC skill.

## What I Learned
During this project I learned:
- how Linux stores authentication activity in /var/log/auth.log  
- how sudo failures appear in logs  
- how Splunk ingests local log files  
- how to build Splunk searches around specific event strings  
- how security events progress from raw logs to actionable detections  

I also reinforced SIEM fundamentals and incident analysis thinking, focusing on how failed login patterns can indicate malicious activity.

## Future Improvements Planned
Planned next iterations for this lab include:
- automated alerting for multiple failed sudo attempts
- dashboards visualizing failed vs successful logins
- correlation of events by username and source IP
- incorporating SSH authentication failure monitoring
- adding Splunk Universal Forwarder instead of local file monitoring
- expanding data sources (syslog, auditd, web server logs)
- enriching events with geolocation and host information

## Screenshots
A dedicated /images folder will contain screenshots including:
- Ubuntu VM running in VirtualBox
- Splunk Enterprise interface
- data input configuration for auth.log
- searches showing failed password attempts
- example raw log events captured in Splunk  

## Conclusion
This project demonstrates full lifecycle security monitoring: environment creation, SIEM deployment, log ingestion, event generation, and detection validation. It directly mirrors real-world SOC analyst workflows and highlights my ability to configure tools, understand log data, and create meaningful detections. This repository serves as a practical portfolio project showing applied blue-team skills with Splunk and Linux authentication monitoring.

## 📸 Screenshots

### Ubuntu VM running in VirtualBox
![Ubuntu VM](images/Screenshot%20from%202026-01-02%2002-31-58.png)

### Splunk detecting failed authentication attempts
![Splunk Web](images/Screenshot%20from%202026-01-02%2002-35-40.png)

### Splunk web interface
![Splunk failed login detection](images/Screenshot%20from%202026-01-02%2002-48-02.png)

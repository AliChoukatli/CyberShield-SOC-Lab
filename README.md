# CyberShield: SOC Honeynet with Microsoft Sentinel
![Azure](https://img.shields.io/badge/Azure-Sentinel-blue?logo=microsoftazure)
![SOC](https://img.shields.io/badge/SOC-Lab-orange)
![KQL](https://img.shields.io/badge/KQL-Queries-green)
[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)



## Overview
This project demonstrates the deployment and monitoring of a **honeynet environment** in Microsoft Azure, integrated with **Microsoft Sentinel** and **Microsoft Defender** for threat detection, incident response, and security visibility.

The goal is to simulate a real-world enterprise SOC (Security Operations Center) scenario by:
- Deploying vulnerable virtual machines and resources in Azure.
- Collecting logs from multiple data sources (VMs, Defender, Network Security Group, etc.).
- Configuring Microsoft Sentinel to detect malicious activities (e.g., RDP brute-force, suspicious logins, impossible travel).
- Creating workbooks, analytics rules, and hunting queries.

This lab is **for educational purposes only** and should not be used in production.

---

## Architecture

![Architecture](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Architecture.png)

- **Resource Group**: `CyberShield-RG`
- **Virtual Network**: `CyberShield-Vnet`
- **Virtual Machines**: Simulated vulnerable Windows/Linux endpoints / SQL Database
- **Entra ID (Azure AD)**: Identity and access management 
- **Microsoft Defender for Cloud**: Provides threat protection
- **Microsoft Sentinel**: SIEM + SOAR for monitoring and incident response
- **Log Analytics Workspace**: Centralized log collection
- **Key Vault**: Secure storage for secrets, keys, and certificates
- 

**Data sources ingested:**
- Security logs from Windows and Linux VMs (including SQL Server logs)
- Microsoft Defender alerts
- Sign-in logs from Azure AD / Entra ID
- NSG flow logs

---

## ⚡ Features

- **Honeynet Deployment**: Exposed VMs to attract malicious traffic.
- **Log Collection**: Centralized in Azure Log Analytics.
- **Sentinel Dashboards**: Custom workbooks for monitoring attacks.
- **Detection Rules**:
  - Unusual Location Sign-in
  - Impossible Travel Detection
  - RDP Brute Force Attempts
- **Threat Hunting Queries** (KQL) to investigate malicious patterns.
- **Incident Response**: Playbooks and automated actions.

---

## 📋 Prerequisites

- Azure subscription with sufficient credits (at least 2 vCPUs + 4GB RAM per VM).
- Permissions to create resource groups, virtual networks, and enable Microsoft Sentinel.
- Basic knowledge of Azure and KQL.

---

# CyberShield SOC Lab - Documentation Overview

* **[Part 1: Environment Setup](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/01-Environment-Setup.md)**
    * 1.1: Resource Groups, Windows/Linux VMs, SQL Server
  
* **[Part 2: Vulnerabilities & Configuration](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/02-Vulnerabilities.md)**  
    * 2.1: RDP/SSH open, Firewall OFF, SQL misconfiguration

* **[Part 3: Attacker VM & Log Generation](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/03-Attacker_VM-%26-Log_Generation.md)**
    * 3.1: Attacker VM, brute force attempts, Event Viewer, Syslog

* **[Part 4: Sentinel Integration](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/04-Log-Ingestion.md)**
    * 4.1: Log Analytics Workspace (LAW) creation  
    * 4.2: Microsoft Sentinel setup & Watchlists  
    * 4.3: Microsoft Defender for Cloud configuration  
    * 4.4: Storage Accounts logging  
    * 4.5: NSG Flow Logs  
    * 4.6: Data Collection Rules (Linux Syslog)  
    * 4.7: Windows Security Events via AMA & DCR  
    * 4.8: Verify collected logs  
    * 4.9: Entra ID logs  
    * 4.10: User creation & activity logs  
    * 4.11: Activity Logs export  
    * 4.12: Additional logs via Storage Accounts & Key Vault

* **[Part 5: Microsoft Sentinel Setup](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/05-Sentinel-Setup.md)**  
    * 5.1: Watchlists, Analytics Rules, Workbooks, Incidents

* **[Part 6: Hardening & Incident Management](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/06-Hardening.md)**  
    * 6.1: Linux & Windows NSG rules, Firewall, Defender for Cloud recommendations  
    * 6.2: Incident management in Sentinel

* **[Part 7: Metrics Before & After Hardening](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/07-Metrics.md)**  
    * 7.1: Security metrics and reporting – Before vs After

* **[Part 8: Compliance](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Documentation/08-Compliance.md)**
    * 8.1: NIST mapping, Defender recommendations

  ---

## 🚀 Deployment Steps

1. **Create Resource Group & VNet**
   ```bash
   az group create --name CyberShield-RG --location eastus
   az network vnet create --name CyberShield-Vnet --resource-group CyberShield-RG --address-prefix 10.0.0.0/16
   ```

2. **Deploy Virtual Machines (Windows & Linux)**
   - Use `Standard_B2s` size (2 vCPUs, 4GB RAM)
   - Expose RDP/SSH for honeypot simulation

3. **Enable Microsoft Defender for Cloud**
   - Turn on Defender plans (Servers, Storage, etc.)

4. **Configure Microsoft Sentinel**
   - Create Log Analytics Workspace
   - Onboard Microsoft Sentinel
   - Connect data sources (Azure AD, Defender, VM logs)

5. **Add Analytics Rules & Workbooks**
   - Import prebuilt detection rules
   - Create custom dashboards

6. **Simulate Attacks**
   - Run brute-force attempts from external machines
   - Generate failed login events

7. **Investigate in Sentinel**
   - Use Hunting Queries (KQL)
   - Validate detections in dashboards

---

## 📊 KQL Queries

| Metric | Description | Query |
|--------|-------------|-------|
| 🔹 Windows Security Events | Count all Windows security events | `SecurityEvent \| count` |
| 🔹 Linux Security Events | Count all Linux security events | `Syslog \| count` |
| 🔹 Security Alerts | Count all security alerts except custom ones | `SecurityAlert \| where DisplayName !startswith "CUSTOM" \| count` |
| 🔹 Security Incidents | Count all security incidents | `SecurityIncident \| count` |
| 🔹 Failed RDP Logins | Detect brute force attempts on Windows | `SecurityEvent \| where EventID == 4625 \| summarize FailedLogins=count() by Account, IPAddress, bin(TimeGenerated, 1h)` |
| 🔹 Failed SSH Logins | Detect brute force attempts on Linux | `Syslog \| where Facility=="auth" \| where SyslogMessage startswith "Failed password for" \| summarize FailedLogins=count() by SourceIP, HostName, bin(TimeGenerated,1h)` |
| 🔹 MSSQL Authentication Failures | Detect brute force attempts on SQL Server | `Event \| where EventLog=="Application" \| where EventID==18456 \| summarize FailedLogins=count() by AttackerIP, Computer, bin(TimeGenerated,1h)` |
| 🔹 Impossible Travel Detection | Detect suspicious logins | `SigninLogs \| summarize Locations=makeset(Location) by UserPrincipalName, bin(TimeGenerated,1h) \| where array_length(Locations) > 1` |
| 🔹 Unusual Location Sign-ins | Detect logins from unusual locations | `SigninLogs \| summarize count() by UserPrincipalName, Location, bin(TimeGenerated,1h) \| order by count_ desc` |

---

## Deliverables

- **Documentation** (README + screenshots)
- **KQL queries** for threat hunting
- **Analytics rules** for detection
- **Dashboards & workbooks** for monitoring

---

## 🏁 Conclusion

This project provided hands-on experience with deploying a honeynet in Microsoft Azure and integrating it with Microsoft Sentinel and Microsoft Defender.  
Through simulated attacks and monitoring, we gained insights into:

- Threat detection and incident response in a SOC-like environment.  
- The importance of security hardening and compliance (NIST SP 800-53).  
- How Microsoft’s cloud-native tools (Defender, Sentinel, Workbooks, Analytics Rules) enhance visibility and security posture.  

🔒 This lab demonstrates how organizations can leverage Azure’s ecosystem to build a proactive and resilient security strategy aligned with **Zero Trust principles**.  

---

## ⚠️ Disclaimer
This project is intended **for educational and training purposes only**. Do not expose production systems or sensitive environments to malicious traffic.

---

## References & Inspiration

- Inspired by [kphillip1's Azure SOC Honeynet Lab](https://github.com/kphillip1/azure-soc-honeynet)

---

## 👤 Author
**Ali Choukatli**  
CyberSec Enthusiast | IAM | SOC | Endpoint Security

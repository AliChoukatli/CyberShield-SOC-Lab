# CyberShield-SOC-Honeynet-Sentinel

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)



## Overview
This project demonstrates the deployment and monitoring of a **honeynet environment** in Microsoft Azure, integrated with **Microsoft Sentinel** for threat detection, incident response, and security visibility.

The goal is to simulate a real-world enterprise SOC (Security Operations Center) scenario by:
- Deploying vulnerable virtual machines and resources in Azure.
- Collecting logs from multiple data sources (VMs, Defender, Network Security Group, etc.).
- Configuring Microsoft Sentinel to detect malicious activities (e.g., RDP brute-force, suspicious logins, impossible travel).
- Creating workbooks, analytics rules, and hunting queries.

This lab is **for educational purposes only** and should not be used in production.

---

## Architecture

- **Resource Group**: `CyberShield-RG`
- **Virtual Network**: `CyberShield-Vnet`
- **Virtual Machines**: Simulated vulnerable Windows/Linux endpoints
- **Microsoft Defender for Cloud**: Provides threat protection
- **Microsoft Sentinel**: SIEM + SOAR for monitoring and incident response
- **Log Analytics Workspace**: Centralized log collection

**Data sources ingested:**
- Security logs from VMs
- Microsoft Defender alerts
- Sign-in logs from Azure AD
- NSG flow logs

---

## Features

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

## Prerequisites

- Azure subscription with sufficient credits (at least 2 vCPUs + 4GB RAM per VM).
- Permissions to create resource groups, virtual networks, and enable Microsoft Sentinel.
- Basic knowledge of Azure and KQL.

---

## Deployment Steps

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

## Example KQL Queries

- **Failed RDP Logins**
  ```kql
  SecurityEvent
  | where EventID == 4625
  | where AccountType == "User"
  | summarize count() by Account, IPAddress, bin(TimeGenerated, 1h)
  ```

- **Impossible Travel Detection**
  ```kql
  SigninLogs
  | summarize makeset(Location) by UserPrincipalName, bin(TimeGenerated, 1h)
  | where array_length(makeset(Location)) > 1
  ```

---

## Deliverables

- **Documentation** (README + screenshots)
- **KQL queries** for threat hunting
- **Analytics rules** for detection
- **Dashboards & workbooks** for monitoring

---

## Disclaimer
This project is intended **for educational and training purposes only**. Do not expose production systems or sensitive environments to malicious traffic.

---

## Author
**Ali Choukatli**  
Cybersecurity Enthusiast | SOC & Endpoint Security | Azure Sentinel Projects

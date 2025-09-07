# 🔴 04 - Log Analytics Workspace & Sentinel Integration

## 📝 Introduction

In this chapter, we will configure a comprehensive logging and monitoring environment in Azure using **Log Analytics Workspace (LAW)** and **Microsoft Sentinel**.  
The goal is to centralize logs from multiple sources, including Windows and Linux virtual machines, Microsoft Entra ID, storage accounts, and Key Vaults, and to enable advanced threat detection and analysis.  

By the end of this chapter, you will have:  
- Created a Log Analytics Workspace and integrated it with Microsoft Sentinel.  
- Configured Microsoft Defender for Cloud to collect security and operational data.  
- Set up Data Collection Rules (DCRs) for both Linux and Windows VMs.  
- Enabled Entra ID and Azure resource logs for centralized monitoring.  
- Prepared additional log sources via Storage Accounts and Key Vaults.  

This setup is the foundation for effective security monitoring, alerting, and incident response in a modern Azure environment.

---

## 🚀 5.1. Create Log Analytics Workspace

1. Go to **Azure Portal** → *Log Analytics Workspaces* → **Create**.  
2. Enter a name (e.g., `LAW-CyberShield`) → **Review + Create**.

## 🚀 5.2. Connect Microsoft Sentinel

1. Go to **Microsoft Sentinel** → **Create**.  
2. Select your **Log Analytics Workspace**.  
3. Create a **Watchlist**:
   - Name: `geoip`  
   - Source Type: *Local File*  
   - Download & upload the [CSV file](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Sentinel/geoip-summarized.csv) 
   - SearchKey: `Network` → **Create**

![Watchlist_Sentinel](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/watchlist_Sentinel.png)

4. Go to **Logs**.
5. Run the query to check if events are collected:
```kusto
_GetWatchlist("geoip")
|count
```
> You should see **54,803 entries**.

![Count_geoip](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/geoip_count.png)

--- 

## 🚀 5.3 Microsoft Defender for cloud

1. Go to **Microsoft Defender for Cloud** → *Environment Settings*.  

![Env_Setting](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Environment_Setting.png)

2. Select your workspace `LAW-CyberShield` → **Defender Plans** → turn **Servers** ON → **Save**.  

3. Go to **Data Collections** → select **All Events** → **Save**.  

4. Return to **Environment Settings** → *Azure Subscription* → **Defender Plans** → turn ON:
   - Servers  
   - Databases  
   - Storage  
   - Key Vault 

![Def_plans](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Def_Plans.png)
 
5. Go to **Settings** → *Continuous Export* → Log Analytics Workspace tab → select all **Exported data types**.  
6. Select **Resource Group** (`RG-CyberShield`) and **Target Workspace** (`LAW-CyberShield`) → **Save**.  

![Continuous_Export](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Continuous%20export.png)

---- 

## 🚀 5.4 Storage accounts

1. Go to **Azure** → *Storage Accounts* → **Create**.  
2. Select your **Resource Group**, enter a unique name → **Review + Create**. 

![Storage creation](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/storage_creation.png)

--- 

## 🚀 5.5 NSG Flow Logs

1. Go to **Azure** → *Network Security Groups (NSG)*.  
2. Select the **Windows VM** → *NSG Flow Logs* → **Create**.  
3. Add target resources: **Linux VM** and **Windows VM**.  
4. Go to **Analytics** tab → set interval to **Every 10 min** → **Create**.  

![NSG_flow](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/NSG_Flow.png)

----

## 🚀 5.6 Data Collection Rules (Linux) 

1. Go to **Data Collection Rules** → **Create**.  
2. Enter a name (e.g., `DCR-Linux-Syslog`) → select **Linux** as platform.  
3. Go to **Resources** → select your **Linux VM**.  
4. Go to **Collect & Deliver** → **Add Data Source** → choose **Syslog**.  
5. Set destination to **Azure Monitor Logs** → target your LAW → **Create**.

![DCR_Linux_review](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/DCR_Linux_review.png)

---

## 🚀 5.7 Collect Windows Security Events via AMA and DCR


Microsoft now uses **Azure Monitor Agent (AMA)** with **Data Collection Rules (DCR)** for Windows Security Events.

### Steps:

1. Go to the **Defender Portal**.  
2. Search for **Windows Security Events via AMA** → **Install**.  
3. Open **Connector Page** → **Create Data Collection Rule**.  
4. Configure the DCR:
   - **Rule Name**: `Windows-Security-Events`  
   - **Subscription**: your Azure subscription  
   - **Resource Group**: `RG-CyberShield`  

![Windows_Events_Basic](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Windows_Events_Basic.png)

5. Go to **Resources** → select your **Windows VM(s)**.  

![Windows_Events_Resource](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Windows_Events_Resource.png)

6. Go to **Collect** → select **All Security Events** (ensures both Success & Failure audits are collected).  

![Windows_Events_Review](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Windows_Events_Review.png)
  
7. Click **Review + Create** → **Create**.

---

## 🚀 5.8. Verify Logs in Log Analytics Workspace

### Windows 

1. Open your **Log Analytics Workspace** (e.g., `LAW-RG-lab`).
2. Go to **Logs**.
3. Run the query to check if events are collected:

```kql
SecurityEvent
| take 10
```

![Security_Event_KQL](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Security_Event_KQL.png)

### Linux 

1. Open your **Log Analytics Workspace** (e.g., `LAW-RG-lab`).
2. Go to **Logs**.
3. Run the query to check if events are collected:

```kql
Syslog
| take 10
```

![syslog_KQL](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/syslog_KQL.png)

---

## 🚀 5.9 Entra ID Logs

1. Go to **Microsoft Entra ID** → *Diagnostic Settings* → **+ Add Diagnostic Setting**.  
2. Name the setting and select the following logs:
   - AuditLogs  
   - SignInLogs  
3. Set the destination: **Send to Log Analytics Workspace** → **Save**.  

![DS](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/DS.png)

---

## 🚀 5.10 Create a User in Entra ID

1. Go to **Microsoft Entra ID** → **+ New User** → **Create User**.  
2. Go to the user (e.g., `test`) → **Assigned Roles** → **Add Assignment** → **Global Administrator**.  
   > This will generate authentication logs.  
3. Open an **incognito tab** → log in with the new user:  
   - Attempt 2 wrong passwords and 1 correct password  
   - Create a new password → sign in.  
4. Return to **Log Analytics Workspace** → **Logs** to verify the generated logs.  

![auditlogs_test](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/auditlogs_test.png)

---

## 🚀 5.11 Export Activity Logs

1. Go to **Monitor** → *Activity Log* → **Export Activity Logs**.  
2. Add a **Diagnostic Setting** → select the following categories:  
   - Administrative  
   - Security  
   - ServiceHealth  
   - Alert  
   - Recommendation  
   - Policy  
   - Autoscale  
   - ResourceHealth  
3. Send to **Log Analytics Workspace** → **Save**.  

- ![Azure_Activity](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Azure_Activity.png)

---

## 🚀 5.12 Additional Logs via Storage Accounts & Key Vault

### Storage Accounts

1. Go to **Storage Accounts** → *Monitoring* → **Diagnostic Settings** → select **Blob** → **+ Add Diagnostic Setting**.  
2. Go back → *Data Storage* → **Containers** → create a test container named `test`.  

![DS_blob](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/DS_blob.png)

---

### Key Vaults


1. Go to **Key Vaults** → **+ Create** → enter a name (e.g., `test20250828`).

![Keyvault_secret](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/keyvault_secret.png)

2. Access configuration → select **Vault Access Policy** → choose a user (e.g., Ali Choukatli) → **Review + Create**.
3. Select the Key Vault → **Secrets** → **Generate/Import** → provide a name & secret value.
4. Select the secret → **Show Secret Value**.  
   > This action will trigger logs.

![Secret_Value](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Secret_value.png)

5. Go to **Log Analytics Workspace** → run the query:

![StorageBlobLogs_KQL](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/StorageBlobLogs_KQL.png)

---


## ✅ Conclusion

By completing this chapter, you have successfully implemented a robust **logging and monitoring architecture** in Azure.  

Your environment now enables:  
- Centralized collection of security and operational logs.  
- Advanced threat detection and analysis using Microsoft Sentinel.  
- Continuous monitoring of both Windows and Linux systems, Entra ID activities, and Azure resources such as Storage Accounts and Key Vaults.  

This setup provides the necessary visibility and control to detect, investigate, and respond to security incidents efficiently, forming a critical part of a proactive **Security Operations Center (SOC)** in Azure.

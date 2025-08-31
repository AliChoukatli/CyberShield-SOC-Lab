# Log Analytics Workspace & Sentinel Integration:

- Go to Azure > Log Analytics workspace > Create
- Give it a Name > Create

- Go to Microsoft Sentinel
- Create > Select your Workspace
- Go to 'Watchlist' -> New 
- Name : 'geoip'
- Source Type : Local File
- Download and upload the csv File (put link for csv file)
- SearchKey : Network -> Create

![Watchlist_Sentinel](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/watchlist_Sentinel.png)

> You should see 54803

```kusto
_GetWatchlist("geoip")
|count
```
![Count_geoip](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Count_geoip.png)

--- 

Microsoft Defender for cloud

- Go to Microsoft for Cloud -> Environment settings 

![Env_Setting](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Environment_Setting.png)

- Select LAW-CyberShield > Defender Plans and put 'on' for Servers > Save

- Go to data Collections tab > select All Events > Save

- Go back to Environment Settings > Click on Azure Subscription > Defender Plans
- Select Servers + Databases + Storage + Key Vault > ON

![Def_plans](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Def_plans.png)
 
- on Setting > Select Continuous export > Log Analytics workspace tab
- Select All 'Exported data types'

- Resource group > Select your Ressource Group (Eg: RG-CyberShield)
- Select Target workspace > EG: LAW-CyberShield > Save

![Continuous_Export](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Continuous%20export.png)

---- 

Storage accounts

- on Azure, Go to Storage accounts
- Select Create
- Choose the ressource group and choose a name 
- Click Review + Create

![Storage creation](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/storage_creation.png)

--- 

- Go to NSG ( Network Security Groups)

- Go to Azure > NSG
- Select the Windows VM > NSG flow logs
- Create
- Select target Ressources > Select Linux vm and Windows vm 

![NSG_flow](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Flow.png)

- Analytics tab > Every 10 min
- Create


----

# Data Collection Rules ( we will create a DCR for Linux to add it to Azure monitor) 

- Go to Data Collection Rules > Create
- Give it a name
-  Select Linux as a platform
- Go to Resources tab > Select linux-vm
- Go to Collect & Deliver tab > Add Data Source > Select Syslog
- On Destination : Select Azure monitor logs to your LAW > Create

---

# Collect Windows Security Events via Azure Monitor Agent (AMA) and Data Collection Rule (DCR)

For Windows, Microsoft changed the way we create the Windows Security Events using and the AMA agent and Data Collection Rule.

## Step 1: Create Data Collection Rule (DCR)

1. GO to Defender Portal : https://security.microsoft.com/sentinel/ba4ff38f-0dee-45af-8b8b-0d92f1d17290/rg-cybershield/law-cybershield/contenthub?tid=60448f2a-c3b7-4368-b20e-916bda32b12d
2. Search  for **Windows Security Events** > **Windows Security Events via AMA** > Install
3. Slelect on **Windows Security Events via AMA** > Open Connector Page
4. Click **Create Data Collection Rule**.
5. Configure the DCR:
   - **Rule Name**: e.g., `Windows-Security-Events`
   - **Subscription**: choose your Azure subscription
   - **Resource Group** : RG-CyberShield

![Windows_Events_Basic](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Windows_Events_Basic.png)

6. Go to the **Resources** tab:
   - Select your **Windows VM(s)** to monitor

![Windows_Events_Resource](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Windows_Events_Resource.png)

7. Go to the **Collect** tab:
   - Select **All Security Events** (this ensures Success and Failure audits are collected)

  ![Windows_Events_Review](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Windows_Events_Review.png)
  
8. Click **Review + Create**, then **Create**.

---

## Verify Logs in Log Analytics Workspace for Windows-vm

1. Open your **Log Analytics Workspace** (e.g., `LAW-RG-lab`).
2. Go to **Logs**.
3. Run the query to check if events are collected:

```kql
SecurityEvent
| take 10
```

![Security_Event_KQL](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Security_Event_KQL.png)

## Verify Logs in Log Analytics Workspace for Linux-vm

1. Open your **Log Analytics Workspace** (e.g., `LAW-RG-lab`).
2. Go to **Logs**.
3. Run the query to check if events are collected:

```kql
Syslog
| take 10
```
![syslog_KQL](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/syslog_KQL.png)


---

# Entra ID Logs

- Search for Microsoft Entra ID > Diagnostic Settings
- click on + Add diagnostic setting
- Give it a name
- Select
   - AuditLogs
   - SignInLogs
- Destination details > Send to Log Analytics workspace

![DS](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/DS.png)

- Save

# Create a user on Entra ID 

- Search for Microsoft Entra ID
- + New User > Create a new user
- Create
- Go to the user : test > Assigned Roles > Add Assignments > Global Administrator

> this will create an auth log

- Open a new incognito tab > go to Azure portal
- Connect with the user principal name (EG: test@cybershieldlab.onmicrosoft.com)
- do 2 wrong password and 1 correct password
- Create a new password > Sign in.

- Go back to Azure > Log Analytic Workspace > Log

![auditlogs_test](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/auditlogs_test.png)

- On Azure > Search for Monitor > Activity log > Export Activity Logs
- Add Diagnostic setting > Select
   - Administrative
   - Security
   - ServiceHealth
   - Alert
   - Recommendation
   - Policy
   - Autoscale
   - ResourceHealth
- Send to Log Analytics workspace

- GO to Log Analytics workspace > Logs

- ![Azure_Activity](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Azure_Activity.png)

---

# Let's create more logs throgh Storage accounts & Key Vault

## Storage Accounts
- On Azure Search for 'Storage accounts ' > Monitoring > Diagnostic Settings
- Click On 'blob'
- + Add diagnostic setting

![DS_blob](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/DS_blob.png)

- Go back to 'Storage accounts' > Select your storage > Data Storage > Containers
- Create a test container > Add Container > Name : test

---

## Key Vaults
- On Azure Search for Key Vaults > + Create
- Give it a name (Eg: test20250828)
- on Access configuration tab > select Vault access policy - Select the user (Eg: Ali Chokatli)
- Review Create
- Select the key vault ( eg: test20250828) > Object > Secrets > Generate/import
- Name it & give it a secret value

![Keyvault_secret](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/keyvault_secret.png)

- select the secret key
- CLick on Show Secret Value
> This will trigger some logs

![Secret_Value](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Secret_value.png)

- Go to Log Analytics workspace > type :
  'StorageBlobLogs'

![StorageBlobLogs_KQL](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/StorageBlobLogs_KQL.png)

---

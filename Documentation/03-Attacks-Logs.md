# 🔴 03 - Attacker & Logs

## 📝 Introduction
In this section, we will:  

1. Create an **Attacker VM**.  
2. Simulate attacks on Windows and Linux VMs.  
3. Generate logs (RDP, SSH, SQL).  
4. Configure SQL Server to send logs to Event Viewer.  
5. Review logs on the VMs.  

This prepares the environment for log ingestion and analysis in Microsoft Sentinel.

---

## 🚀 1. Attacker Machine Setup

- Create a new resource group: **RG-Attacker**
- Create a Windows VM named **attacker-vm**
- On the network tab, create a new VNet for the attacker VM (e.g., `attacker-vm-vnet`)
- Review & Create.

![Attacker_Vm_creation](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker-vm-creation.png)
     
### RDP Attacker VM using Public IP

![attacker_RDP_Win](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker_RDP_Win.png)

## 🚀 2. Attack Simulation & Log Generation

### 2.1 On Windows VM

1. From **Attacker VM**, attempt **RDP connections with wrong credentials**.  
2. Install **SSMS** and try **2 wrong SQL logins** followed by **1 successful login**.  

![attacker-ssms-fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker-ssms-fail.png)

--- 

### 2.2 On Linux VM

- From Linux VM, attempt **SSH connections with wrong passwords** to generate logs

![attacker-ssh-connect-fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker-ssh-connect-fail.png)

----


## 🚀 3. SQL Server Audit Configuration (Windows VM)

To enable SQL Server to send logs to Windows Event Viewer:

1. Open the [Microsoft documentation page](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16) for guidance.


Copy the registry key:

```pgsql
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services/EventLog/Security
```

![reg_fullcontrol](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/reg_fullcontrol.png)

2. Open Registry Editor, search and paste the key.
3. Enable auditing using auditpol:

```bash
auditpol /set /subcategory:"application generated" /success:enable /failure:enable
```

![audipol](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/audipol_Cmd.png))

4. In SSMS, enable logging for both successful and failed logins:

![SSMS_Success_Fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/SSMS_Success_Fail.png))


## 🚀 4. Viewing Logs

### 4.1 On Windows VM

- Open Event Viewer to check:
  - Failed RDP attempts
  - Successful and failed SQL logins
    
![Event_view_sql_fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Event_view_sql_fail.png)

![event_attacker-login_fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/event_attacker-login_fail.png)


### 4.2 On Linux VM

```bash
cd /var/log
cat auth.log | grep password
```
- Check failed and successful SSH login attempts

![cat_auth_log](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/cat_auth_log.png)

![cat_auth_log_fail_success](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/cat_auth_log_fail_success.png)



---

## ✅ Conclusion

- The Attacker VM is operational.
- Simulated attacks generate logs on both Windows and Linux.
- SQL Server is configured to send logs to Event Viewer.
- Logs are ready for ingestion and correlation in Microsoft Sentinel for security incident analysis and detection.


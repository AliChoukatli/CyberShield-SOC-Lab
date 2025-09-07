# 🔴 04 – Log Ingestion & Viewing

## 📝 Introduction

This section covers how to configure SQL Server and the VMs to generate logs, simulate attacks, and view them for monitoring purposes.

---

## 🚀 4.1 Configure SQL Server Audit to Event Viewer

To enable SQL Server to send logs to Windows Event Viewer:

1. Open the [Microsoft documentation page](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16) for guidance.
2. Copy the registry key:  
      HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security
3. Open **Registry Editor**, paste the key in the search bar.
3. Open **Registry Editor**, paste the key in the search bar.

![reg_fullcontrol](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/reg_fullcontrol.png)

5. Enable auditing using `auditpol` in Command Prompt:
```bash
  auditpol /set /subcategory:"application generated" /success:enable /failure:enable
```

![audipol](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/audipol_Cmd.png)
6. open SSMS -> put your crenedential
- right click on windows-vm - Properties -> Security
- Select Both failed and successful logins so we can see all logs
  
![SSMS_Success_Fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/SSMS_Success_Fail.png)
 
7. click disconnect and reconnect.

---

## 🚀 4.2 Generate Attacker Logs

### Windows Attacker VM
- RDP from `Attacker_VM` to `Windows_VM` using **wrong credentials** to generate logs.

![Attacker_RDP_fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/RDP_Attacker-vm.png)

- On the attacker machine, install **SSMS** and try to connect with **2 wrong passwords** and **1 correct password**.


![attacker-ssms-fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker-ssms-fail.png)

--- 

### Linux VM
- Generate logs by attempting **SSH connections** with wrong passwords from the Linux VM.

![attacker-ssh-connect-fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker-ssh-connect-fail.png)

---

## 🚀 4.3 View Logs on Windows VM
- Open **Event Viewer** on Windows VM.  
- You should see the generated logs from **SQL Server** and **failed login attempts**.
  
- go to event viewer, and we can see some logs from our previous exercice 

![Event_view_sql_fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Event_view_sql_fail.png)

![event_attacker-login_fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/event_attacker-login_fail.png)


---



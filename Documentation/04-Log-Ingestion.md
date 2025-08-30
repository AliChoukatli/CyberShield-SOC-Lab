To make SQL Server send logs to event viewer
- Go to : https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16
- Copy : HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security
- Open Registery editor and paste the key on the Search bar.
- Right click on Security tab and select Permissions -> Add -> Type : Network Service -> Full Control

![reg_fullcontrol](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/reg_fullcontrol.png)

- go back to the website on the learn page : go to the section 'Configure the audit object access setting in Windows using auditpol and copy the statement to enable auditing from SQL Server.

```
  auditpol /set /subcategory:"application generated" /success:enable /failure:enable
```
and paste it on command prompt 

![audipol](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/audipol_Cmd.png)

- open SSMS -> put your crenedential
- right click on windows-vm - Properties -> Security
- Select Both failed and successful logins so we can see all logs
  
![SSMS_Success_Fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/SSMS_Success_Fail.png)

- click disconnect and reconnect

---

## Attacker Logs 

- let's RDP from Attacker_VM to Windows_VM and put wrong credentials so we can generate some logs

- Download SSMS on the attacker machine and try to connect with 2 wrong password and 1 correct password to generate logs.

![attacker-ssms-fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker-ssms-fail.png)

--- 

## Linux logs 

- now let's generate some logs from our linux, go to powershell and connect ssh with wrong passwords.

![attacker-ssh-connect-fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker-ssh-connect-fail.png)

----

# Let's go back to our Windows-vm and look for the logs 
- go to event viewer, and we can see some logs from our previous exercice 

![Event_view_sql_fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Event_view_sql_fail.png)

![event_attacker-login_fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/event_attacker-login_fail.png)


---

## now let's connect on linux machine and look for logs

type: cd /var/log 

type: cat auth.log | grep password to view logs

![cat_auth_log](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/cat_auth_log.png)

![cat_auth_log_fail_success](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/cat_auth_log_fail_success.png)


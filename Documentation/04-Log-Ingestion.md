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

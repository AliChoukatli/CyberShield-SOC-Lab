# Create a ressource groups
   - Go to Ressource Groups > Create > Name your Ressource Group : (EG: RG-CyberShield)

# Windows Vm Creation
   - Go to Virtual machines > Create > name it windows-vm
   - on network tab > Create a new vnet > eg : CyberShield-Vnet > Review & Create
     
![windows-vm-creation](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/windows-vm-Creation.png)

# Linux Vm Creation
   - Go to Virtual machines > Create > name it linux-vm
   - On network tab > choose the same vnet for windows > eg : CyberShield-Vnet > Review & Create

![linux-vm-creation](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/linux-vm-creation.png)

# SQL Database

- on the windows vm, now we will downlaodSQL Database  : go to : https://www.microsoft.com/en-us/evalcenter/download-sql-server-2022, then dowload the exe file

- download media -> ISO -> download

- Open folder  > right click on SQLServer2022-x64 -> Mount

![SQL_Mount](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/SQL_mount.png)

- click Setup 
- once done, Go to installation -> New SQL Server standalone -> Next 
- At Feature Selection tab, check Instance Features -> Database Engine Services -> Next
- At Database Engine Configuration -> Mixed Mode (SQL Server authentication) -> Put your password -> Add Current User -> Next -> Install
- Once done, go to : https://learn.microsoft.com/en-us/ssms/sql-server-management-studio-ssms -> Download & install SSMS 
- Restart the computer

---

# Attacker Machine

- Go to Virtual machines > Create > > Create a new Ressource Group as 'RG-Attacker' and name the machine as 'attacker-vm'
- On network tab > create a new vnet for windows > eg : attacker-vm-vnet > Review & Create
     
# RDP Attacker Windows machine using its Public IP

![attacker_RDP_Win](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker_RDP_Win.png)

# 🔴 01 Environment-Setup.

## 📝 Introduction
This chapter covers the initial setup of our Azure Honeynet SOC Lab environment.  
We will create the necessary Resource Groups, deploy Windows and Linux virtual machines, configure networking with Virtual Networks (VNet), and install SQL Server along with SQL Server Management Studio (SSMS).  
Additionally, an attacker VM will be prepared for simulation exercises.  
All steps are accompanied by screenshots to ensure clarity and reproducibility.
---

## 1.1 Resource Groups
### 🚀  Step to Create your Resource Groups (e.g., RG-CyberShield)
- Go to Ressource Groups > Create > Name your Ressource Group : (EG: RG-CyberShield)

---

## 1.2 Windows VM Creation
### 🚀  Steps to create Windows VM, network, vnet
- Go to Virtual machines > Create > name it windows-vm
- on network tab > Create a new vnet > eg : CyberShield-Vnet > Review & Create
     
![windows-vm-creation](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/windows-vm-Creation.png)

### RDP Windows machine using its Public IP

![Win_IP](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/win_ip.png)

- Go to your host and go to cmd and ping your windows-vm

![ping_windows](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/ping-windows.png)

---

## 1.3 Linux VM Creation
### 🚀 Steps to create Linux VM on same VNet
- Go to Virtual machines > Create > name it linux-vm
- On network tab > choose the same vnet for windows > eg : CyberShield-Vnet > Review & Create

![linux-vm-creation](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/linux-vm-creation.png)

### we will ping our linux machine using the public IP

![ping-linux](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/ping-linux.png)

now let's connect to our linux vm using ssh:

![ssh_linux_connect](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/ssh_linux_connect.png)

---

## 1.4 SQL Database Installation
### 🚀 Steps - Download & Installation
- Download SQL Server + ISO + Mount
- Install SQL Server & SSMS
- on the windows vm, now we will downlaodSQL Database  : go to : https://www.microsoft.com/en-us/evalcenter/download-sql-server-2022, then dowload the exe file
- download media -> ISO -> download
- Open folder  > right click on SQLServer2022-x64 -> Mount

![SQL_Mount](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/SQL_mount.png)

### 🚀 Steps - SQL Server Configuration
- Open the SSMS folder
- click Setup 
- once done, Go to installation -> New SQL Server standalone -> Next 
- At Feature Selection tab, check Instance Features -> Database Engine Services -> Next
- At Database Engine Configuration -> Mixed Mode (SQL Server authentication) -> Put your password -> Add Current User -> Next -> Install
- Once done, go to : https://learn.microsoft.com/en-us/ssms/sql-server-management-studio-ssms -> Download & install SSMS 
- Restart the computer

---

## 1.5 Attacker VM Setup
### 🚀 Steps - Creation of RG-Attacker, the VM attacker-vm & Network
- Go to Virtual machines > Create > > Create a new Ressource Group as 'RG-Attacker' and
- Name the machine as 'attacker-vm'
- On network tab > create a new vnet for windows > eg : attacker-vm-vnet > Review & Create
     
### RDP Attacker Windows machine using its Public IP

![attacker_RDP_Win](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker_RDP_Win.png)

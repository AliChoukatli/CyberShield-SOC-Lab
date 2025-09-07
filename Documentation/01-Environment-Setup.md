# 🔴 01 Environment Setup

## 📝 Introduction

This chapter covers the initial setup of our Azure Honeynet SOC Lab environment.  
We will create the necessary Resource Groups, deploy Windows and Linux virtual machines, configure networking with Virtual Networks (VNet), and install SQL Server along with SQL Server Management Studio (SSMS).  
Additionally, an attacker VM will be prepared for simulation exercises.  
All steps are accompanied by screenshots to ensure clarity and reproducibility.

---

## 🚀 1.1 Resource Groups Creation (e.g., RG-CyberShield)

- Go to Ressource Groups > Create > Name your Ressource Group : (EG: RG-CyberShield)

---

## 🚀 1.2 Windows VM Setup

- Go to Virtual machines > Create > name it windows-vm
- on network tab > Create a new vnet > eg : CyberShield-Vnet > Review & Create
     
![windows-vm-creation](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/windows-vm-Creation.png)

### RDP Windows machine using its Public IP

![Win_IP](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/win_ip.png)

- Go to your host and go to cmd and ping your windows-vm

![ping_windows](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/ping-windows.png)

---

## 🚀 1.3 Linux VM Setup

- Go to Virtual machines > Create > name it linux-vm
- On network tab > choose the same vnet for windows > eg : CyberShield-Vnet > Review & Create

![linux-vm-creation](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/linux-vm-creation.png)

### we will ping our linux machine using the public IP

![ping-linux](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/ping-linux.png)

now let's connect to our linux vm using ssh:

![ssh_linux_connect](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/ssh_linux_connect.png)

---

## 🚀 1.4 SQL Database Download & Installation

- Download SQL Server + ISO + Mount
- Install SQL Server & SSMS
- on the windows vm, now we will downlaodSQL Database  : go to : https://www.microsoft.com/en-us/evalcenter/download-sql-server-2022, then dowload the exe file
- download media -> ISO -> download
- Open folder  > right click on SQLServer2022-x64 -> Mount

![SQL_Mount](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/SQL_Mount.png)

### SQL Server Configuration

- Open the SSMS folder
- click Setup 
- once done, Go to installation -> New SQL Server standalone -> Next 
- At Feature Selection tab, check Instance Features -> Database Engine Services -> Next
- At Database Engine Configuration -> Mixed Mode (SQL Server authentication) -> Put your password -> Add Current User -> Next -> Install
- Once done, go to : https://learn.microsoft.com/en-us/ssms/sql-server-management-studio-ssms -> Download & install SSMS 
- Restart the computer

---

## 🚀 1.5 Attacker VM Setup

- Create a new resource group: **RG-Attacker**
- Create a Windows VM named **attacker-vm**
- On the network tab, create a new VNet for the attacker VM (e.g., `attacker-vm-vnet`)
- Review & Create.

![Attacker_Vm_Creation](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker-vm-creation.png)
     
### RDP Attacker Windows machine using its Public IP

![attacker_RDP_Win](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker_RDP_Win.png)

---

## ✅ Conclusion
The environment setup is now complete.  
We have a fully provisioned lab with Windows and Linux VMs, a configured SQL Server instance, and an attacker VM ready for log generation exercises.  
This foundation ensures that subsequent chapters on vulnerabilities, log ingestion, and Microsoft Sentinel integration can be executed smoothly and consistently.

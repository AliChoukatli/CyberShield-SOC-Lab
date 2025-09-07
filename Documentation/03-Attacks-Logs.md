# 🔴 03 - Attacker & Logs

## 📝 Introduction : 
This section demonstrates how we simulate attacks from the Attacker VM and generate logs on both Windows and Linux VMs.

---

## 🚀 3.1 Attacker Machine Setup

- Create a new resource group: **RG-Attacker**
- Create a Windows VM named **attacker-vm**
- On the network tab, create a new VNet for the attacker VM (e.g., `attacker-vm-vnet`)
- Review & Create.
     
### RDP Attacker VM using Public IP

![attacker_RDP_Win](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker_RDP_Win.png)

## 🚀 3.2 Attacker Logs - Windows

- RDP from **Attacker_VM** to **Windows_VM** using wrong credentials to generate logs
- Install SSMS on the attacker machine and attempt 2 wrong logins followed by 1 correct login

![attacker-ssms-fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker-ssms-fail.png)

--- 

## 🚀 3.3 Attacker Logs - Linux

- From Linux VM, attempt SSH connections with wrong passwords to generate logs

![attacker-ssh-connect-fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/attacker-ssh-connect-fail.png)

----
## 3.4 Viewing Logs on Windows VM

- Open **Event Viewer** to see generated logs from previous exercises. 

![Event_view_sql_fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/Event_view_sql_fail.png)

![event_attacker-login_fail](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/event_attacker-login_fail.png)

---

## 🚀 3.5 Viewing Logs on Linux VM

Now, let's access the Linux VM and inspect the logs that were generated during the previous activities.

```bash
cd /var/log 
cat auth.log | grep password to view logs
```
![cat_auth_log](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/cat_auth_log.png)

![cat_auth_log_fail_success](https://github.com/AliChoukatli/CyberShield-SOC-Lab/blob/main/Screenshots/cat_auth_log_fail_success.png)

---

##  ✅ Conclusion

In this section, we successfully set up the Attacker VM and performed simulated attacks on both the Windows and Linux VMs. We generated logs from:

- Failed and successful RDP and SQL login attempts on Windows
- Failed SSH login attempts on Linux

All these logs were collected and verified on the respective systems. This prepares the environment for log ingestion and correlation in Microsoft Sentinel, allowing us to detect, analyze, and respond to potential security incidents in a realistic lab scenario.

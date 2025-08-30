# 02 - Vulnerabilities Setup

## Introduction
In this chapter, we intentionally make our lab machines vulnerable to simulate attacks.  
We will modify Network Security Group (NSG) rules, open critical ports, and disable firewalls to allow attack simulations from our attacker VM.

---

## 2.1 Windows VM Vulnerabilities

### Remove existing RDP rule

- Delete the RDP port 3389 rule to allow unrestricted access.

![NSG_Windows_Delete_RDP_Rule](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Windows_Delete_RDP_Rule.png)

### Create new rule for any source
- Allow RDP from any source.

![NSG_Win_Rule_any](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Win_Rule_any.png)

### Test RDP Access
- Connect to the Windows VM using its Public IP:

![Win_IP](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/win_ip.png)

### Disable Windows Firewall
- Turn off Firewall for all profiles using Advanced Firewall Settings.

![win_turnoff_firewall](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/win_turnoff_firewall.png)

---

## 2.2 Linux VM Vulnerabilities
### Remove SSH rule
- Delete the default SSH rule.

![NSG_Windows_Delete_SSH_Rule](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Windows_Delete_SSH_Rule.png)

### Create a rule allowing all traffic
- Allow any traffic to Linux VM.

![NSG_Linux_Rule_any](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_linux_Rule_any.png)

---

## Conclusion
Both Windows and Linux VMs are now intentionally vulnerable.  
These configurations will allow us to generate attack logs and test Microsoft Sentinel detection rules in the following chapters.

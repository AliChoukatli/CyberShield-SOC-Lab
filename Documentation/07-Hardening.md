
# Hardening

## Linux Rule 
- we will chose to fix this : CUSTOM: Brute Force ATTEMPT - Linux Syslog)
  
![Eg_Brute Force ATTEMPT - Linux Syslog](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Eg_Brute%20Force%20ATTEMPT%20-%20Linux%20Syslog.png)

- Go to Network Security Groups (NSG)
- Let's edit the rule 110 by changing Source ip from * (Any) to My IP Address

![Linux_Rule_fix](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Linux_Rule_fix.png)

## Windows Rule
- we can do the same thing with Windows NSG
- Let's edit the rule 100 by changing Source ip from * (Any) to My IP Address

![Windows_Rule_fix](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Windows_Rule_fix.png)

---

## Mitigate Firewall

- Let's RDP our Windows_VM to mitigate the firewall
- Set Firewall State to : ON for all profiles


# Manage Incidents

- we can manage eg: the windows incident ( Brute Force Attempt)
- Fill the infos > Save 
  
![Manage_Incident](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Manage_Incident.png)
  
---

# For more Hardening - Microsoft Defender for Cloud Recommendation 

- Go to Microsoft Defender for Cloud > Security Posture > Recommendations

![Defender_Recommendations](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Defender_Recommendations.png)

An Example : Azure Backup should be enabled for virtual machines

![recomm_Backup](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/recomm_Backup.png)


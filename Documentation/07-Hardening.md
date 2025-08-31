# 🔴 Hardening & Incident Management

## 📝 Introduction

Hardening your cloud environment is a critical step in reducing attack surfaces and mitigating potential security threats. This section focuses on implementing practical hardening measures for both Linux and Windows virtual machines, configuring network security rules, managing incidents, and leveraging Microsoft Defender for Cloud recommendations to ensure a robust security posture.

---

## Linux NSG Rule Hardening

To mitigate brute force attempts on Linux systems:

1. Identify the custom alert: **CUSTOM: Brute Force ATTEMPT - Linux Syslog**.
  
![Eg_Brute Force ATTEMPT - Linux Syslog](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Eg_Brute%20Force%20ATTEMPT%20-%20Linux%20Syslog.png)

2. Go to **Network Security Groups (NSG)** in Azure.
3. Edit the rule **110** and change **Source IP** from `* (Any)` to **your own IP address**.

![Linux_Rule_fix](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Linux_Rule_fix.png)

## Windows NSG Rule Hardening

Similarly, for Windows systems:

1. Navigate to the Windows NSG.
2. Edit the rule **100** and change **Source IP** from `* (Any)` to **your own IP address**.

![Windows_Rule_fix](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Windows_Rule_fix.png)

---

## Firewall Mitigation

To ensure Windows VM is properly secured:

1. RDP into your **Windows VM**.
2. Open **Windows Defender Firewall**.
3. Set the **Firewall State** to **ON** for all profiles (Domain, Private, Public).

---

## Managing Incidents

After hardening, you can manage incidents in Microsoft Defender / Sentinel:

1. Select an incident (e.g., **Windows Brute Force Attempt**).
2. Fill in the required information and **Save** the incident.

![Manage_Incident](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Manage_Incident.png)

> Proper NSG rule hardening and firewall configuration help mitigate brute force attacks, while incident management ensures security events are tracked and resolved efficiently.

---

# Additional Hardening - Microsoft Defender for Cloud Recommendations

To further secure your environment, leverage the security recommendations provided by **Microsoft Defender for Cloud**:

1. Navigate to **Microsoft Defender for Cloud** → **Security Posture** → **Recommendations**.

![Defender_Recommendations](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Defender_Recommendations.png)

2. Review the suggested actions and implement relevant recommendations to strengthen your cloud security posture.

   **Example:** Enable **Azure Backup** for virtual machines to ensure data protection and recovery.

![recomm_Backup](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/recomm_Backup.png)

---

## ✅ Conclusion

By applying these hardening techniques and following the security recommendations provided by Microsoft Defender for Cloud, you can significantly enhance the protection of your Azure environment. Regular review of incidents, continuous monitoring, and adherence to best practices are essential to maintain a secure and resilient cloud infrastructure.

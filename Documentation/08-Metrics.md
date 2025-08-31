# 🔴 Metrics Before Hardening / Security Controls

The table below presents the security metrics collected over a 24-hour period in our unprotected environment:

**Monitoring Period:**  
Start Time: 2025-08-27 14:38  
Stop Time: 2025-08-28 14:38  

![Metric_Before](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Metric_before.png)

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 267594
| Syslog                   | 8135
| SecurityAlert            | 92
| SecurityIncident         | 141


## Metrics After Hardening / Security Controls

The table below shows the security metrics collected over a 24-hour period after implementing hardening measures and security controls:

**Monitoring Period:**  
Start Time: 2025-08-28 16:00  
Stop Time: 2025-08-29 16:00  

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 108439
| Syslog                   | 41
| SecurityAlert            | 3
| SecurityIncident         | 3

![Metric_After](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Metric_after.png)

**✅Observation:**  
The hardening and security measures significantly reduced the number of security events, syslog messages, alerts, and incidents, demonstrating an improved security posture and a lower attack surface.


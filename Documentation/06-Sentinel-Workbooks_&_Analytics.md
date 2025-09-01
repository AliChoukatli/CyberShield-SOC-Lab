# 🔴 06 - Sentinel Workbooks & Analytics

## 📝 Introduction
This section covers the creation and configuration of Microsoft Sentinel Workbooks, the import of custom JSON queries for monitoring authentication events, and the setup of analytics rules. By following these steps, you can visualize and monitor brute force attempts and other suspicious activities across Windows, Linux, and SQL Server environments. This provides a centralized view of security events, enabling proactive threat detection and response.

---

## 🚀 6.1 Sentinel Workbook

1. Open [Microsoft Sentinel Workbooks](https://security.microsoft.com/sentinel/ba4ff38f-0dee-45af-8b8b-0d92f1d17290/rg-cybershield/law-cybershield/workbooks?tid=60448f2a-c3b7-4368-b20e-916bda32b12d).  
2. Click **Add Workbook** → **Edit**.  
3. Remove existing content.  
4. Click **Add** → **Add Data Source + Visualization** → **Advanced Editor**.  
5. Import the JSON file for each use case below and click **Done Editing**.

### 6.1.1 Windows RDP Authentication Failures
   
 ```json
  {
  "type": 3,
  "content": {
    "version": "KqlItem/1.0",
    "query": "let GeoIPDB_FULL = _GetWatchlist(\"geoip\");\nlet WindowsEvents = SecurityEvent;\nWindowsEvents | where EventID == 4625\n| order by TimeGenerated desc\n| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)\n| project TimeGenerated, Account, AccountType, Computer, EventID, Activity, IpAddress, LogonTypeName, network, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, \" (\", countryname, \")\");\n",
    "size": 3,
    "timeContext": {
      "durationMs": 2592000000
    },
    "queryType": 0,
    "resourceType": "microsoft.operationalinsights/workspaces",
    "visualization": "map",
    "mapSettings": {
      "locInfo": "LatLong",
      "locInfoColumn": "countryname",
      "latitude": "latitude",
      "longitude": "longitude",
      "sizeSettings": "EventID",
      "sizeAggregation": "Count",
      "opacity": 0.8,
      "labelSettings": "friendly_location",
      "legendMetric": "EventID",
      "legendAggregation": "Count",
      "itemColorSettings": {
        "nodeColorField": "EventID",
        "colorAggregation": "Sum",
        "type": "heatmap",
        "heatmapPalette": "greenRed"
      }
    }
  },
  "name": "query - 0"
}

```
   - you will see the map > click Save

![windows-rdp-auth-fail_Map](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/windows-rdp-auth-fail_Map.png)


### 6.1.2 Linux SSH Authentication Failures

```json
{
  "type": 3,
  "content": {
    "version": "KqlItem/1.0",
    "query": "let GeoIPDB_FULL = _GetWatchlist(\"geoip\");\nlet IpAddress_REGEX_PATTERN = @\"\\b\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\b\";\nSyslog\n| where Facility == \"auth\"\n| where SyslogMessage startswith \"Failed password for\"\n| order by TimeGenerated desc\n| project TimeGenerated, SourceIP = extract(IpAddress_REGEX_PATTERN, 0, SyslogMessage), DestinationHostName = HostName, DestinationIP = HostIP, Facility, SyslogMessage, ProcessName, SeverityLevel, Type\n| evaluate ipv4_lookup(GeoIPDB_FULL, SourceIP, network)\n| project TimeGenerated, SourceIP, DestinationHostName, DestinationIP, Facility, SyslogMessage, ProcessName, SeverityLevel, Type, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, \" (\", countryname, \")\");",
    "size": 3,
    "timeContext": {
      "durationMs": 2592000000
    },
    "queryType": 0,
    "resourceType": "microsoft.operationalinsights/workspaces",
    "visualization": "map",
    "mapSettings": {
      "locInfo": "LatLong",
      "locInfoColumn": "countryname",
      "latitude": "latitude",
      "longitude": "longitude",
      "sizeSettings": "latitude",
      "sizeAggregation": "Count",
      "opacity": 0.8,
      "labelSettings": "friendly_location",
      "legendMetric": "friendly_location",
      "legendAggregation": "Count",
      "itemColorSettings": {
        "nodeColorField": "latitude",
        "colorAggregation": "Count",
        "type": "heatmap",
        "heatmapPalette": "greenRed"
      }
    }
  },
  "name": "query - 0"
}

```
   - you will see the map > click Save

![linux-ssh-auth-fail_Map](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/linux-ssh-auth-fail_Map.png)


### 6.1.3 MSSQL Authentication Failures

```json
{
  "type": 3,
  "content": {
    "version": "KqlItem/1.0",
    "query": "let GeoIPDB_FULL = _GetWatchlist(\"geoip\");\nlet IpAddress_REGEX_PATTERN = @\"\\b\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\b\";\n// Brute Force Attempt MS SQL Server\nEvent\n| where EventLog == \"Application\"\n| where EventID == 18456\n| project TimeGenerated, AttackerIP = extract(IpAddress_REGEX_PATTERN, 0, RenderedDescription), DestinationHostName = Computer, RenderedDescription\n| evaluate ipv4_lookup(GeoIPDB_FULL, AttackerIP, network)\n| project TimeGenerated, AttackerIP, DestinationHostName, RenderedDescription, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, \" (\", countryname, \")\");",
    "size": 3,
    "timeContext": {
      "durationMs": 2592000000
    },
    "queryType": 0,
    "resourceType": "microsoft.operationalinsights/workspaces",
    "visualization": "map",
    "mapSettings": {
      "locInfo": "LatLong",
      "locInfoColumn": "countryname",
      "latitude": "latitude",
      "longitude": "longitude",
      "sizeSettings": "latitude",
      "sizeAggregation": "Count",
      "opacity": 0.8,
      "labelSettings": "friendly_location",
      "legendMetric": "friendly_location",
      "legendAggregation": "Count",
      "itemColorSettings": {
        "nodeColorField": "latitude",
        "colorAggregation": "Sum",
        "type": "heatmap",
        "heatmapPalette": "greenRed"
      }
    }
  },
  "name": "query - 0"
}
```
- click Save

---
## 🚀 6.2 Analytics 

1. Navigate to [Microsoft Sentinel Analytics](https://security.microsoft.com/sentinel/ba4ff38f-0dee-45af-8b8b-0d92f1d17290/rg-cybershield/law-cybershield/analytics?tid=60448f2a-c3b7-4368-b20e-916b-0d92f1d17290).  
2. Import the JSON file containing your analytics rules.

![Analytics](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Analytics.png)


## 🚀 6.3 Monitor Incidents

1. Go to **Microsoft Defender** → **Incidents & Alerts** → **Incidents**.  
2. You should see brute force attempts for both Windows and Linux.

![Incidents](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Incidents.png)

- Example of Incident:

![Incident_Bruteforce_attempt](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Incident_Bruteforce_attempt.png)

![Incident_Bruteforce_IPs](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Incident_Bruteforce_IPs.png)

---

## ✅ Conclusion
After completing the workbook and analytics setup, your Sentinel environment will display real-time maps and dashboards highlighting authentication failures and potential attacks. The integration with Microsoft Defender ensures that incidents are automatically tracked, providing actionable insights for security operations. This setup lays the foundation for continuous monitoring and effective incident response in your Azure environment.

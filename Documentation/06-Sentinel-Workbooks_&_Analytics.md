# # 🔴 06 - Sentinel Workbooks & Analytics

- Go to [https://security.microsoft.com/sentinel/ba4ff38f-0dee-45af-8b8b-0d92f1d17290/rg-cybershield/law-cybershield/workbooks?tid=60448f2a-c3b7-4368-b20e-916bda32b12d]
- Add Workbook > Edit
- Remove what is there
- Add > Add data source + visualization > Advanced Editor
- we will add some json file ( link ) for :

  1) windows-rdp-auth-fail
   - Copy this json script + Done editing
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

 2) linux-ssh-auth-fail
   - Copy this json script + Done editing

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

3) mssql-auth-fail
- Copy this json script + Done editing
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

# Analytics 

- Go to Analtyics or Microsoft Defender Analytics ( Recent Method): (https://security.microsoft.com/sentinel/ba4ff38f-0dee-45af-8b8b-0d92f1d17290/rg-cybershield/law-cybershield/analytics?tid=60448f2a-c3b7-4368-b20e-916bda32b12d)
- import the json file 

![Analytics](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Analytics.png)

- Go to Microsoft Defender > Incidents & Alerts > Incidents
- we can see Both Brute Force ATTEMPT for Windows & Linux

![Incidents](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Incidents.png)

- Example of Incident:

![Incident_Bruteforce_attempt](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Incident_Bruteforce_attempt.png)

![Incident_Bruteforce_IPs](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Incident_Bruteforce_IPs.png)

# Workbooks and Geo Locations

## What are Workbooks and Why They Are Useful?

Workbooks provide a flexible canvas for data analysis and the creation of rich visual reports within the Azure portal. They allow users to tap into multiple data sources from across Azure and combine them into unified interactive experiences.

These workbooks are useful because they enable users to create personalized reports and share insights with others, making data analysis and reporting a collaborative experience. Furthermore, the reports can be saved and shared with others, promoting transparency and knowledge transfer.

## What are Geo Locations?

Geo Locations in workbooks refer to the visual representation of geographically based data. They are useful because they allow users to visualize and analyze data in relation to physical locations. This can provide valuable insights for businesses with multiple locations, for services that are location-sensitive, or for analyzing user behavior across different regions. These insights can be instrumental in strategic planning, decision-making, and optimizing resource allocation.

## Threat Map Creation

The circles in the workbooks indicate the volume of attacks by their IP’s Geo Location pulled from a latitudinal/longitudinal Watchlist of the major cities all over the globe.

## My Geo Location Workbooks

These are my 4 workbooks, with their KQL and Geo Location

<img src="Workbooks and Geo Locations Pics Folder/MSW 1.png">

# linux-ssh-auth-fail.json

Geo Location:

<img src="Workbooks and Geo Locations Pics Folder/MSW 2.png">

KQL:

```sql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let IpAddress_REGEX_PATTERN = @"\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b";
Syslog
| where Facility == "auth"
| where SyslogMessage startswith "Failed password for"
| order by TimeGenerated desc
| project TimeGenerated, SourceIP = extract(IpAddress_REGEX_PATTERN, 0, SyslogMessage), DestinationHostName = HostName, DestinationIP = HostIP, Facility, SyslogMessage, ProcessName, SeverityLevel, Type
| evaluate ipv4_lookup(GeoIPDB_FULL, SourceIP, network)
| project TimeGenerated, SourceIP, DestinationHostName, DestinationIP, Facility, SyslogMessage, ProcessName, SeverityLevel, Type, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, " (", countryname, ")");
```

# mssql-auth-fail.json

Geo Location: 

<img src="Workbooks and Geo Locations Pics Folder/MSW 3.png">

KQL:

```sql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let IpAddress_REGEX_PATTERN = @"\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b";
// Brute Force Attempt MS SQL Server
Event
| where EventLog == "Application"
| where EventID == 18456
| project TimeGenerated, AttackerIP = extract(IpAddress_REGEX_PATTERN, 0, RenderedDescription), DestinationHostName = Computer, RenderedDescription
| evaluate ipv4_lookup(GeoIPDB_FULL, AttackerIP, network)
| project TimeGenerated, AttackerIP, DestinationHostName, RenderedDescription, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, " (", countryname, ")");
```

# nsg-malicious-allowed-in.json

Geo Location:

<img src="Workbooks and Geo Locations Pics Folder/MSW 4.png">

KQL:

```sql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let MaliciousFlows = AzureNetworkAnalytics_CL 
| where FlowType_s == "MaliciousFlow"
| order by TimeGenerated desc
| project TimeGenerated, FlowType = FlowType_s, IpAddress = SrcIP_s, DestinationIpAddress = DestIP_s, DestinationPort = DestPort_d, Protocol = L7Protocol_s, NSGRuleMatched = NSGRules_s;
MaliciousFlows
| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)
| project TimeGenerated, FlowType, IpAddress, DestinationIpAddress, DestinationPort, Protocol, NSGRuleMatched, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, " (", countryname, ")")
```

# windows-rdp-auth-fail.json

Geo Location:

<img src="Workbooks and Geo Locations Pics Folder/MSW 5.png">

KQL:

```sql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent;
WindowsEvents | where EventID == 4625
| order by TimeGenerated desc
| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)
| project TimeGenerated, Account, AccountType, Computer, EventID, Activity, IpAddress, LogonTypeName, network, latitude, longitude, city = cityname, country = countryname, friendly_location = strcat(cityname, " (", countryname, ")");
```

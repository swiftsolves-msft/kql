# KQL Workshop 101 | Examples and Exercises

  

## 'Search' operator

```Kql
//example 1
search "10.0.1.5"

//eaxample 2
SecurityEvent | search "Guest"
```

## Search -  enhance, enhance, enhance

```Kql
// does this data exist ?
search "vnevado-win11t"

// where and how often does the data exist ? 
search "vnevado-win11t" 
| summarize EventCount = count() by $table 
| order by EventCount desc 

// why does it exist ?
search in (DeviceNetworkEvents) "vnevado-win11t"
```

## 'Where' -  exercise

```Kql
//example 1
DeviceLogonEvents
| where Timestamp > ago(14d)

//example 2
DeviceLogonEvents
| where Timestamp > ago(14d) and ActionType == "LogonFailed" // Failed logon

//example 3
DeviceLogonEvents
| where Timestamp > ago(14d)
| where ActionType == "LogonFailed" 
| where Protocol =~ "ntlm"

//example 4
DeviceLogonEvents | where ActionType in ("LogonSuccess", "LogonAttempted")

//example 5
DeviceNetworkEvents | where ipv4_is_match(LocalIP, "10.0.0.0/8")
```

## 'extend' operator

```Kql
//example 1
SecurityEvent | extend ComputerNameLength = strlen(Computer) // extend defaults column to end

//example 2 
SecurityEvent
| extend ComputerNameLength = strlen(Computer)
| project-reorder ComputerNameLength, * // bring the extneded column first or closer
```

## 'extend' exercise

```Kql
//example 1
DeviceInfo
| where OSPlatform == "Linux" or OSPlatform contains "WindowsServer"
| distinct DeviceName
| summarize count()
| extend ['DfS P1 Pricing'] = count_ * 5
| extend ['DfS P2 Pricing'] = count_ * 15

//example2
SecurityEvent 
| where EventID in (4624, 4625) 
| extend rgroup = extract("resourcegroups/(.*)/providers",1,_ResourceId)

//example 3
SecurityEvent 
| where EventID in (4624, 4625) 
| extend rgroup = split(_ResourceId, "/",4)[0]

//example 4
SecurityEvent 
| where EventID in (4624, 4625)
 
//example 5
SecurityEvent 
| where EventID in (4624, 4625) 
| parse _ResourceId with "/subscriptions/" sub "/resourcegroups/" rgroup "/providers" *
```

## Lab 1: Filtering

Find all failed attempts to login events starting 2 weeks ago until 1 week ago that occurred on a computer with name which starts with “Ash”.

  

Hints and guideline:

-   Process creation is Windows event 4688
-   Domain controller names end with “-dc” before going into remaining fqdn elements
-   Create multiple charts by aggregating additional more than one field
-   Search for bin windows in KQL

## 'Union' operator

```Kql
DeviceEvents
| union (DeviceNetworkEvents | where ActionType == "ConnectionSuccess")
```

## 'Join' operator

```Kql
//example 1
DeviceInfo | join (DeviceNetworkEvents | where RemoteIPType == "Public") on DeviceId

// example 2
DeviceInfo
| join kind=inner (
    DeviceNetworkEvents
    | where Timestamp > ago(7d)
    | summarize NetworkEventsCount = count() by DeviceId
) on DeviceId
| project DeviceName, OSPlatform, NetworkEventsCount, LastSeenTimestamp = Timestamp
| order by NetworkEventsCount desc
```

## JSON exercise

```Kql
//example 1
DeviceEvents
| where ActionType == 'PnpDeviceConnected'
| extend entity = parse_json(AdditionalFields)
| extend ClassName = entity.ClassName
| extend DeviceDescription = entity["DeviceDescription"]
| project DeviceName, ClassName, DeviceDescription 

//example 2
DeviceEvents
| where ActionType == 'PnpDeviceConnected'
| project ATDynamic = parse_json(AdditionalFields)
| evaluate bag_unpack(ATDynamic)
```

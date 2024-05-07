# Building a Honeynet + SOC inside Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In my recent project, I constructed a small honeynet within Azure, where I funneled log data from diverse sources into a Log Analytics workspace. This data was then leveraged by Microsoft Sentinel to craft attack maps, initiate alerts, and generate incidents. I commenced by gauging several security metrics within the unsecured environment over a 24-hour period. Following this, I implemented a series of security enhancements to fortify the environment. Subsequently, I conducted another 24-hour measurement of metrics, the outcomes of which are detailed below.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

In crafting my Azure-based mini honeynet, I incorporated several vital elements:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (including 2 Windows and 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Initially, for the "BEFORE" metrics analysis, all resources were deployed and exposed to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, while other resources were equipped with public endpoints, rendering Private Endpoints unnecessary.

Subsequently, for the "AFTER" metrics assessment, I bolstered the Network Security Groups by restricting ALL traffic except for my admin workstation. Moreover, I fortified all other resources with their internal firewalls and the added protection of Private Endpoints.
## Attack Maps Before Hardening / Security Controls

![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/4a8471ee-2535-4628-b736-fac766012ac5)
![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/579103da-6ca1-4c64-bf8d-3fa0ae85cb96)
![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/c2e65728-c84b-4ce6-b2d7-f0502283b538)
![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/66d90f6c-46ce-4d4c-9e30-b2c0848cb0e8)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 4/27/2024 15:58:16 Stop Time  4/28/2024 15:58:16

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 27207
| Syslog                   | 15163
| SecurityAlert            | 10
| SecurityIncident         | 335
| NSG Inbound Allowed      | 2184
| NSG Inbound Blocked      | 0



## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 5/3/2024 13:36:41
Stop Time	 5/4/2024 1:36:41 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8463
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| NSG Inbound Allowed      | 0
| NSG Inbound Blocked      | 623

## Conclusion

Within this Azure-based project, a miniature honeynet was established, accompanied by the integration of log sources into a Log Analytics workspace. Leveraging Microsoft Sentinel, alerts and incidents were promptly triggered based on the ingested logs. Notably, metrics were initially gauged within the insecure environment prior to the implementation of security measures. Subsequently, after the application of these measures, a subsequent measurement was conducted. Remarkably, the number of security events and incidents experienced a significant reduction post-implementation, underscoring the efficacy of the security controls.

Furthermore, it's essential to recognize that in scenarios where network resources are heavily utilized by regular users, there's a likelihood of generating more security events and alerts within the 24-hour period following the implementation of security controls.

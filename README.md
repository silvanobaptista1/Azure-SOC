# Building a Honeynet + SOC inside Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls

![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/4a8471ee-2535-4628-b736-fac766012ac5)
![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/579103da-6ca1-4c64-bf8d-3fa0ae85cb96)
![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/c2e65728-c84b-4ce6-b2d7-f0502283b538)
![image](https://github.com/silvanobaptista1/Azure-SOC/assets/169019817/66d90f6c-46ce-4d4c-9e30-b2c0848cb0e8)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 4/27/2024 15:58:16
Stop Time  4/28/2024 15:58:16

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

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

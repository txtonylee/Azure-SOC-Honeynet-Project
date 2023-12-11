# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/WaqBUSa.png)

## Introduction

In this project, I created a mini **honeynet** in Azure, gathering information from different sources and organizing it in a Log Analytics workspace. Microsoft Sentinel was established to analyze this data, helping to create visual attack maps, trigger alerts, and handle security incidents. To evaluate the effectiveness of our security measures, I monitored key security metrics in the less secure environment for 24 hours, then implemented additional security controls to strengthen the setup. After another 24 hours, I compared the results. The metrics we'll discuss include

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
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the "BEFORE" phase, all resources were initially set up and fully exposed on the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls configured with broad access, and all other resources were deployed with public endpoints visible to the Internet, Private Endpoints are not implemented.

Moving to the "AFTER" phase, we enhanced the Network Security Groups by restricting ALL traffic except for connections from my admin workstation. Additionally, all other resources received protection from their built-in firewalls and were shielded using Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/WK7WVXf.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/62PrOwO.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/Oaeibm4.png)<br>
![MsSQL Auth Failures](https://i.imgur.com/27jVsrL.png)<br>

## Attack Maps After Hardening / Security Controls
NSG Allowed Inbound Malicious Flows map has a significantly reduced attack after hardening.
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/ZZT6XIc.png)<br>

All map queries but NSG-malicious-allowed-in returned no results due to no instances of malicious activity for the 24 hours after hardening.
![No queries](https://i.imgur.com/dlZZL2q.png)<br>

## Microsoft Sentinel (Maps, Incidents, Alerts)
![Incidents](https://i.imgur.com/dqouhni.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-15 17:04:29
Stop Time 2023-11-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4358
| Syslog                   | 2345
| SecurityAlert            | 6
| SecurityIncident         | 73
| AzureNetworkAnalytics_CL | 103


## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 2023-11-18 15:37
Stop Time	2023-11-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 110
| Syslog                   | 344
| SecurityAlert            | 1
| SecurityIncident         | 2
| AzureNetworkAnalytics_CL | 0

## Results Comparing Before & After Hardening / Security Controls
The following table shows the metrics comparing before and after the hardening/security controls:

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | -97.48%
| Syslog                   | -85.33%
| SecurityAlert            | -83.33%
| SecurityIncident         | -97.26%
| AzureNetworkAnalytics_CL | -100.00%

## Conclusion
In this cybersecurity project, we set up a small protective network, or honeynet, using Microsoft Azure. We organized the log sources neatly into a Log Analytics workspace, and Microsoft Sentinel was utilized to identify potential issues and create incident reports based on the logs. We also measured certain metrics in the less secure environment, both before and after we put in place additional security measures. The key takeaway is that once these security controls were applied, we observed a significant drop in the number of security events and incidents, demonstrating their effectiveness.

It's important to note that if the network resources were heavily used by regular users, there might have been more security events and alerts during the 24 hours following the implementation of these security controls.

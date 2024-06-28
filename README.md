# Building a SOC + Honeynet in Azure (Live Traffic)
 ![image](https://github.com/Razone14/Cloud-SOC/assets/158781387/54ea3982-c5a9-4847-81cb-88d9a146b478)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/Razone14/Cloud-SOC/assets/158781387/2a8ae0b0-e4cc-43bf-a50c-a7ca25c7d0fa)


## Architecture After Hardening / Security Controls
![image](https://github.com/Razone14/Cloud-SOC/assets/158781387/887f80c6-077e-486b-9bb0-cabc85919920)


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
![nsg-mallicious-allowed-in-BEFORE](https://github.com/Razone14/Cloud-SOC/assets/158781387/d11c9305-85da-4627-9399-129ab3ef67c1)
<br>
![linux-ssh-suth-fail-BEFORE](https://github.com/Razone14/Cloud-SOC/assets/158781387/436da4b3-5062-477f-8411-94da0dc8af40)
<br>
![windows-rdp-smb-auth-fail-BEFORE](https://github.com/Razone14/Cloud-SOC/assets/158781387/a63421b2-a546-42dc-bdac-5b49a2b91cdc)
<br>
![mssql-auth-fail-BEFORE](https://github.com/Razone14/Cloud-SOC/assets/158781387/cdb855a6-d432-46f7-8878-9cd4ad6bf6f3)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-06-25, 1:14: p.m.
Stop Time 2024-06-26, 1:14: p.m.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 80611
| Syslog                   | 5147
| SecurityAlert            | 6
| SecurityIncident         | 245
| AzureNetworkAnalytics_CL | 103

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-27, 12:46 p.m. 
Stop Time	2024-06-28, 12:46 p.m.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8794
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

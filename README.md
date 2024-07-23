# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC setup](https://github.com/user-attachments/assets/3eda8151-d2f1-4ef9-b356-4ca93bb204e5)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace. These logs are then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. For a deeper dive into the KQL queries used to trigger alerts and create incidents, check out THIS PROJECT. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


## Architecture Before Hardening / Security Controls
![Architecture_BEFORE](https://github.com/user-attachments/assets/d845c928-ffbc-40e1-a0a1-0bf4fe91a3cf)

## Architecture After Hardening / Security Controls
![Architecture_AFTER](https://github.com/user-attachments/assets/d7abbb3f-19e5-4265-8999-67005669fe51)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account (Blob storage)
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, several controls recommendations from NIST 800-53 were implemented. Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.


## Attack Maps Before Hardening / Security Controls
## NSG Allowed Inbound Malicious Flows
![nsg-malicious-allowed-in_BEFORE](https://github.com/user-attachments/assets/1dc5c5e3-1a8c-489b-b8cb-e861fcc5d534)

## Linux SSH Auth Fail
![linux-ssh-auth-fail_BEFORE](https://github.com/user-attachments/assets/e975c829-74e3-4264-adf9-483e1efe5043)

## Windows RDP Auth Fail
![windows-rdp-auth-fail_BEFORE](https://github.com/user-attachments/assets/c0ae90ac-e766-4da9-95f5-a2100703280f)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 7/9/2024 17:13:12

Stop Time 7/10/2024 17:13:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 26017
| Syslog                   | 31025
| SecurityAlert            | 154
| SecurityIncident         | 155
| AzureNetworkAnalytics_CL | 1364

## Attack Maps After Hardening / Security Controls

## Windows RDP Auth Fail
![windows-rdp-auth-fail_AFTER](https://github.com/user-attachments/assets/02081ef8-be6f-42bc-a865-16f921dbc0de)


Linux and NSG map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 7/10/2024 22:12:07

Stop Time	7/11/2024 22:12:07

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 45
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 87
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

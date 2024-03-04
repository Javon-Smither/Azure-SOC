# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/B7u4du8.png)

## Part 1 (Intro)

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. For 24 hours, I measured some security metrics in the insecure environment, while that was happening i applied some security controls to harden the environment, looked at the metrics after another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

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

## Part 2 (Attack Maps Before Hardening / Security Controls)
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/p5usM4J.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/hB9wGyX.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/wTjyPam.png)<br>

## Part 3 (Metrics Before Hardening / Security Controls)

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-20T19:29:10
Stop Time 2024-02-21T19:29:10

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 35713
| Syslog                   | 8736
| SecurityAlert            | 11
| SecurityIncident         | 356
| AzureNetworkAnalytics_CL | 3850

## Part 4 (Attack Maps Before Hardening / Security Controls)

```All map queries actually returned no results due to the fact that there were no instances of malicious activity for the 24 hour period after hardening.```

## Part 5 (Metrics After Hardening / Security Controls)

The following table shows the metrics we measured in our environment for another 24 hours, but AFTER we have applied security controls:
Start Time 2024-02-29T18:04:59
Stop Time	2024-03-01T18:04:59

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9097
| Syslog                   | 22
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Part 6 (Conclusion)

In this project, i constructed a mini Honeynet in Microsoft Azure and log sources were put into the Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is also worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://user-images.githubusercontent.com/65828628/236935173-6cb5f050-376a-4396-97aa-c147d9297f52.gif)

## Summary

For this project, I created a small honeynet on Azure. I gathered logs from different sources and stored them in a Log Analytics workspace. Microsoft Sentinel used this workspace to build attack maps, send alerts, and track incidents. I measured security metrics in the unsecured environment for 24 hours, implemented security measures to strengthen the environment, and then measured metrics for another 24 hours. The following results illustrate the metrics we will present:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://camo.githubusercontent.com/cbabbce9b931457f2d315cf7b54329abb8a236a377baf6f061facbf00a60b650/68747470733a2f2f692e696d6775722e636f6d2f783436474271462e706e67)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://camo.githubusercontent.com/faf06f075bb323f4147f3434106f7843e1090fd678b7432a770a615c1b30a0e6/68747470733a2f2f692e696d6775722e636f6d2f6c4b6f756479582e706e67)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel


In the "BEFORE" metrics, all resources were deployed with open access to the internet. This included Virtual Machines with unrestricted Network Security Groups and built-in firewalls, as well as other resources with public endpoints visible to the internet, eliminating the need for Private Endpoints.





In the "AFTER" metrics, Network Security Groups were strengthened by blocking all traffic except for the admin workstation. Additionally, other resources were safeguarded by their respective built-in firewalls and Private Endpoints.


## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/sMSv7R7.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/BvzC0W0.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/mjLxPmL.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-05-11 4:29:35
Stop Time 2023-05-12 4:29:35

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 45399
| Syslog                   | 9271
| SecurityAlert            | 0
| SecurityIncident         | 366
| AzureNetworkAnalytics_CL | 613

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-05-13 5:23:18
Stop Time	2023-05-14  5:23:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11370
| Syslog                   | 2916
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

A mini honeynet was constructed within Microsoft Azure for this project, integrating log sources into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and create incidents based on the ingested logs. Following the implementation of security measures, metrics were measured in the insecure environment before the controls were applied and again after their implementation. Notably, the application of security controls led to a significant reduction in the number of security events and incidents, demonstrating their effectiveness.

It is important to note that if the network resources were heavily utilized by regular users, it is likely that the implementation of security controls would have generated more security events and alerts within the 24-hour period. However, despite this potential scenario, the number of security events and incidents decreased significantly after the security controls were put in place, further validating their efficacy.

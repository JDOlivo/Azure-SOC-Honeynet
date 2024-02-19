# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet + SOC](https://github.com/JDOlivo/Azure-SOC/assets/146149824/06afca8e-3655-43e2-9427-234ac55e191e)

## Introduction

In this project, I designed and deployed a mini honeynet within an Azure environment. I collected and analyzed log data from diverse resources, consolidating them into a Log Analytics workspace. Microsoft Sentinel was utilized to construct attack maps, generate alerts, and establish incidents based on this log data. The evaluation involved monitoring security metrics within the initial unsecured environment for a 24-hour period. Subsequently, security controls were implemented to fortify the environment. Another 24-hour monitoring period followed, and the outcomes are presented below, showcasing the impact of the security enhancements. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Cloud Honeynet + SOC PG2](https://github.com/JDOlivo/Azure-SOC/assets/146149824/1e4f1efe-5726-4f11-bdae-83f3e9153eb3)

## Architecture After Hardening / Security Controls
![Secured Network](https://github.com/JDOlivo/Azure-SOC/assets/146149824/dc3ebf9d-774e-49af-8344-2c411a5f4d93)

The Azure-based mini honeynet's structure comprises the subsequent components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the "BEFORE" metrics assessment, all resources were initially deployed in a state where they were exposed to the internet. The Virtual Machines had unrestricted Network Security Groups and open built-in firewalls. Additionally, all other resources were deployed with public endpoints accessible over the internet, rendering Private Endpoints unnecessary.

In the "AFTER" metrics assessment, Network Security Groups were strengthened by implementing a policy to block ALL traffic, permitting exceptions only for my admin workstation. Furthermore, all other resources were safeguarded by their built-in firewalls and were accessible via Private Endpoints.


## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in-before](https://github.com/JDOlivo/Azure-SOC/assets/146149824/694ceff7-589a-4ed1-ab4f-916c375c11c9) <br>
![syslog-ssh-auth-fail-before](https://github.com/JDOlivo/Azure-SOC/assets/146149824/cfb49fd6-f444-4e31-b8ae-2c7adcefd40f) <br>
![windows-rdp-auth-fail-before](https://github.com/JDOlivo/Azure-SOC/assets/146149824/c7ea98e2-beee-4ae6-b453-0d3b5a5faf4a) <br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours: <br>
Start Time: 2023-09-26 23:06 <br> 
Stop Time:  2023-09-27 23:06

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 64,925
| Syslog                   | 4417
| SecurityAlert            | 4
| SecurityIncident         | 229
| AzureNetworkAnalytics_CL | 35125

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours after we applied security controls: <br>
Start Time: 2023-10-01 23:22 <br>
Stop Time:	2023-10-02 23:22

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9165
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project we established a mini honeynet within Microsoft Azure, integrating diverse log sources into a centralized Log Analytics workspace. Utilizing Microsoft Sentinel, we orchestrated an alert-triggering mechanism and incident creation based on the logs assimilated. Furthermore, security metrics were evaluated in the initial insecure environment and subsequently post the deployment of security measures. Notably, there was a significant reduction in both security events and incidents following the implementation of these security controls, showcasing their efficacy.

It's pertinent to highlight that if the network resources experienced substantial utilization by regular users, it's plausible that the 24-hour post-security control implementation could have generated a higher volume of security events and alerts.

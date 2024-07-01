# Azure SOC Honeynet Project

# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC Honeynet (1)](https://github.com/0xbythesecond/Azure-SOC-Honeynet-Project/assets/23303634/43177fa9-4746-4f8d-8774-f9aca74b891d)

## Introduction
This project details the creation of a HoneyNet using Microsoft Azure's platform to simulate the actions of a SOC analyst during an attack and how they would harden their system. Using this HoneyNet, I gathered data revealing various bad actors and IP addresses attempting to compromise my system. All credit for assistance goes to the YouTube link below, and visuals are available in the GitHub link. 

***Note***: *I recommend completing this project over several days: dedicate the first day to creating the systems, the second day to gathering initial data, the third day to analyzing the results and hardening the systems, the fourth day to collecting post-hardening data, and the fifth day to comparing the before and after states. If using Azure's free trial, ensure all resources are deleted after the project to avoid costs. Check your account a few days after completion to confirm no charges are incurred.*

## Azure Resources Deployed, Technologies, and Regulations used:
- [Azure Virtual Network](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) (VNet)
- [Azure Network Security Group](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview) (NSG)
- [Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/overview) (2x Windows 10 Pro, 1x Linux Server)
- [Log Analytics Workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-workspace-overview) with Kusto Query Language (KQL) Queries
- [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/basic-concepts) for Secure Secrets Management
- [Azure Storage Account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview) for Data Storage
- [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview) for Security Information and Event Management (SIEM)
- [Microsoft Defender](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction) for Cloud to Protect Cloud Resources
- [Windows Remote Desktop](https://support.microsoft.com/en-us/windows/how-to-use-remote-desktop-5fe128d5-8fb1-7a23-3b8a-41e636865e8c) for Remote Access
- [Command Line Interface](https://www.w3schools.com/whatis/whatis_cli.asp) (CLI) for System Management
- [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.3) for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls (Within the Defender Program)

## Course of Action
- ***Establishing the honeynet:*** To begin this project, ensure you have an account and create two virtual machines: one Windows and one Linux. Ensure both systems are on the same network and in the same time zone. Once both VMs and their connected networks are created, open all ports and traffic to the VMs using Network Security Groups (NSG). *Note: the asterisk is used to indicate all ports.*

- ***Tracking and examination:*** Since the Azure infrastructure allows for seamless ingestion of logs, I created a rule to process logs from different sources into a dedicated Log Analytics workspace. Utilizing the advanced features of Microsoft Sentinel, I generated attack maps (see Map Code folder) within the provided workbooks. To achieve this, download and export the geoip file to Sentinel's watchlist under the same name, as it is necessary for proper functionality. The Xpath text file will be used in the data sources tab within the Log Analytics workspace to retrieve logs from the Windows VM. Regularly monitor these logs and use the attack maps to examine and track any malicious activity effectively.

- ***Tracking and evaluating security metrics:*** I monitored the unsecured environment for a full day, noting important security measurements during that time. This served as a starting point for comparison once I applied security improvements.
-   

- ***Addressing and resolving security incidents:*** Following the resolution of incidents and identification of vulnerabilities, I proceeded to fortify the environment by implementing security best practices and incorporating Azure-specific recommendations.
-   

- ***Analysis after implementing remediation measures:*** An additional 24-hour period was dedicated to the meticulous re-observation of the environment, facilitating a comprehensive evaluation of the security metrics. The resulting data was then meticulously juxtaposed with the initial baseline, enabling a rigorous comparative analysis.
-   

The metrics we will show are:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/gBvHJo4.gif)
In the "BEFORE" measurement phase, it was observed that all resources were initially provisioned with direct internet exposure. The Virtual Machines were configured with open Network Security Groups and permissive built-in firewalls, while other resources were deployed with publicly accessible endpoints, thereby rendering the usage of Private Endpoints unnecessary.
  

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/oQtbais.gif)
In the "AFTER" evaluation stage, the Network Security Groups underwent fortification measures whereby all traffic, with the exception of my administrative workstation, was comprehensively blocked. Additionally, other resources were fortified by leveraging their built-in firewalls alongside the implementation of Private Endpoint functionality.
  

## Attack Maps Before Hardening / Security Controls
The visual representation presented below provides an overview of the assault endeavors targeted at a publicly accessible Microsoft SQL server throughout a span of 24 hours. The plotted data points on the map delineate the precise origins of these attacks or attempted logins.
  
![MSSQL Auth](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/c1e30390-e148-4018-b001-299cb5489ac9) <br />

The depicted attack map elucidates the multitude of syslog authentication failures encountered by the Linux server I provisioned, elucidating the presence of unsanctioned endeavors to gain entry from external sources beyond the confines of the local network. This serves as an emphatic reminder underscoring the indispensability of fortifying Linux servers with robust authentication protocols and diligently scrutinizing system logs to detect and thwart potential intrusions.
  
![Linux SSH Auth](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/8edc8d6f-7062-4ea2-a781-98b07f848152) <br />

The exhibited attack map encapsulates a multitude of RDP (Remote Desktop Protocol) and SMB (Server Message Block) failures, vividly exemplifying the unrelenting endeavors of potential assailants to exploit these specific protocols. The visual depiction accentuates the imperative nature of fortifying remote access and file-sharing services as a means to safeguard against illicit entry and mitigate the looming cyber threats that may ensue.
  
![Windows RDP](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/625ee2e4-b4d2-42f0-9fa3-28c74cb63992) <br />

The illustrated attack map serves as a compelling showcase of the ramifications stemming from the act of leaving the Network Security Group (NSG) unrestricted, thereby facilitating the unhindered ingress of malicious network traffic. This visualization effectively emphasizes the criticality of deploying robust security protocols, including the imposition of stringent NSG rules, as a means to thwart unauthorized entry and mitigate the inherent risks posed by potential threats.
  
![NSG Malicious](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/b9e87cb7-01e9-4ad3-a09c-55960f710640)



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
<br />
`Start Time:` 2024-06-27T22:51:34 <br/>
`Stop Time:` 2024-06-28T22:51:34
| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 55760
| Syslog                   | 1013
| SecurityAlert            | 36
| SecurityIncident         | 73
| AzureNetworkAnalytics_CL | 1781

## Attack Maps After Hardening / Security Controls

  >**Note**: All the attack maps did not show any indication of change after hardening; however, the metrics tell a different story. The metrics highlighted significant improvements in system security post-hardening, revealing the effectiveness of the implemented security measures.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
<br />
`Start Time:` 2024-06-29T23:38:54.<br />
`Stop Time:`	2024-06-30T23:38:54.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Change after Securing Environment
| Metric                                          | Percent
| ----------------------------------------------- | -----
| Security Event (Windows VM)                     |  -100.00%
| Syslog (Linux VM)                               |  -100.00%
| Security Alert (Microsoft Defender for Cloud)   |  -100.00%
| Security Incident (Sentinel Incidents)          |  -100.00%
| NSG Inbound Malicious Flows Allowed             |  -100.00%


## Hardening Stratiges
- Hardening techniques are going to differ between me and you. These are the steps I took to harden my machines to get the results I wanted, but be careful as certain settings can be irreversible.

- I deleted the Attack VM, so I could focus on securing the Linux and Windows VM only
    - Some of the suggestions for securing the systems did have the Attack VM listed
- Go to the Network tab
  - Enable public network access for only selected virtual networks and IP addresses
- Go to your Windows VM setting section
  - Click on Extensions and application
  - Add Mircosoft Antimalware extension
  - Then connect to the Windows through the remote desktop
- I also happened to change the firewall setting
   - exclude all inbound connections
   - Note: This will prevent you from entering through remote desktop once applying this rule
- Create a subnet for both the Linux and Windows VM
    - Go to Virtual Networks
    - Head to the setting sections
    - Click on Subnets
    - Add 2 subnets to the configurations you want for both the Linux and Windows VM
- Disk Encryption
  - [Link](https://learn.microsoft.com/en-us/azure/virtual-machines/disks-enable-host-based-encryption-portal?tabs=azure-powershell&WT.mc_id=Portal-Microsoft_Azure_Security#set-up-your-disk-encryption-set)
- There is some hardening process that I had to exempt because the reasoning was that it was either for the attack VM or something else:
  - Take for instance Log Analytics agent should be installed on virtual machines
      - I needed to do this for my Windows VM however I wasn't able to since "Log Analytics agent should be installed on virtual machines"
      - So an exemption was needed
- Enable a backup
  - [Link](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-monitor-and-troubleshoot#monitor-in-the-dashboard)


## Reflection
This project was both challenging and insightful, offering a glimpse into the daily operations of a Security Operations Center (SOC). Witnessing my work come together despite encountering errors was incredibly rewarding. The project highlighted the significant issues that can arise in an insured environment if it is not secured promptly. One of the most enlightening aspects was observing the state of an insecure system after 24 hours and then seeing the results after securing it, which underscored the importance of timely security measures. Throughout the project, I identified the locations of attackers, with some surprising results. The logs provided extensive information about the attackers, including their IP addresses and actions. This experience helped me understand the critical role of a SOC analyst in securing vulnerabilities before thousands of potential attackers can exploit them.

## Conclusion
This project involved the establishment of a mini honeynet within the Microsoft Azure platform, where diverse log sources were seamlessly integrated into a dedicated Log Analytics workspace. Microsoft Sentinel played a pivotal role in proactively generating alerts and initiating incidents based on the logs ingested. Notably, comprehensive metrics were diligently measured in the vulnerable environment prior to the implementation of security controls, followed by a subsequent assessment after fortifying the infrastructure. The remarkable outcome emerged as a significant reduction in the frequency of security events and incidents, which undeniably attested to the efficacy of the implemented security measures.

-

It is important to acknowledge that if the network's resources were extensively utilized by regular users, it is conceivable that a greater number of security events and alerts could have been generated within the 24-hour timeframe subsequent to the enforcement of the security controls.

## KQL Queries

| Metric                                       | Query                                                                                                                                            |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Start/Stop Time                              | range x from 1 to 1 step 1<br>\| project StartTime = ago(24h), StopTime = now()                                                                  |
| Security Events (Windows VMs)                | SecurityEvent<br>\| where TimeGenerated>= ago(24h)<br>\| count                                                                                   |
| Syslog (Linux VMs)                           | Syslog<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                                         |
| SecurityAlert (Microsoft Defender for Cloud) | SecurityAlert<br>\| where DisplayName !startswith "CUSTOM" and DisplayName !startswith "TEST"<br>\| where TimeGenerated >= ago(24h)<br>\| count |
| Security Incident (Sentinel Incidents)       | SecurityIncident<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                               |
| NSG Inbound Malicious Flows Allowed          | AzureNetworkAnalytics_CL<br>\| where FlowType_s == "MaliciousFlow" and AllowedInFlows_d > 0<br>\| where TimeGenerated >= ago(24h)<br>\| count    |


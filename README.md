# Azure SOC Honeynet Project

# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC Honeynet (1)](https://github.com/0xbythesecond/Azure-SOC-Honeynet-Project/assets/23303634/43177fa9-4746-4f8d-8774-f9aca74b891d)

## Introduction
This project details the creation of a HoneyNet using Microsoft Azure's platform to simulate the actions of a SOC analyst during an attack and how they would harden their system. Using this HoneyNet, I gathered data revealing various bad actors and IP addresses attempting to compromise my system. I may revisit this project at a later date to further experiment with the firewall and will add any new data to the repository. All credit for assistance goes to the YouTube link below, and visuals are available in the GitHub link.

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

- ***Tracking and evaluating security metrics:*** I left an unsecured environment for a full day and then reviewed the results, taking note of the metrics being measured. This served as the point of comparison before applying any hardening techniques. The data gathered during this period provided a baseline to evaluate the effectiveness of the security measures implemented later.

- ***Addressing and resolving security incidents:*** After reviewing the vulnerabilities, I examined various recommendations for security protocols. Based on these recommendations and the incidents observed, I implemented several security measures to address the identified weaknesses. Additionally, I wanted to see the effects of blocking inbound traffic on system security, so I included this as part of the security measures.
  - *Note:* Do not block all inbound traffic. This was my first time experimenting with a firewall, and I wanted to see the consequences of such an action. Now that I know what happens, I advise against blocking all inbound traffic. Instead, I suggest looking at other options for hardening the firewall.

- ***Analysis after implementing remediation measures:*** After waiting an additional day, we reexamined the environment, reviewed it, and took note of the results of the hardening. I then compared these results to the initial vulnerability baseline to evaluate the effectiveness of the implemented security measures.

The metrics we will show are:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/gBvHJo4.gif)
During the "BEFORE" measurement phase, it was evident that all resources were provisioned with direct internet exposure. The Virtual Machines were set up with open Network Security Groups and permissive built-in firewalls. Additionally, other resources had publicly accessible endpoints, making the use of Private Endpoints redundant.


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/oQtbais.gif)
In the "AFTER" evaluation stage, the Network Security Groups were reinforced to block all traffic except from my administrative workstation. Furthermore, other resources were secured by utilizing their built-in firewalls and implementing Private Endpoint functionality.
  

## Attack Maps Before Hardening / Security Controls
The visual representation below offers an overview of the attack attempts on a publicly accessible Microsoft SQL server over a 24-hour period. The plotted data points on the map indicate the exact origins of these attacks or login attempts.
  
![MSSQL Auth](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/c1e30390-e148-4018-b001-299cb5489ac9) <br />

The depicted attack map illustrates numerous syslog authentication failures experienced by the Linux server I deployed, revealing unauthorized attempts to gain access from external sources outside the local network. This highlights the critical need to strengthen Linux servers with robust authentication protocols and to meticulously monitor system logs to detect and prevent potential intrusions.

  
![Linux SSH Auth](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/8edc8d6f-7062-4ea2-a781-98b07f848152) <br />

The exhibited attack map illustrates numerous RDP (Remote Desktop Protocol) and SMB (Server Message Block) failures, vividly showcasing the persistent efforts of potential attackers to exploit these specific protocols. This visual representation emphasizes the critical importance of securing remote access and file-sharing services to protect against unauthorized entry and mitigate potential cyber threats.
  
![Windows RDP](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/625ee2e4-b4d2-42f0-9fa3-28c74cb63992) <br />

The illustrated attack map serves as a compelling showcase of the consequences of leaving the Network Security Group (NSG) unrestricted, allowing unrestricted ingress of malicious network traffic. This visualization effectively underscores the importance of implementing robust security protocols, such as enforcing stringent NSG rules, to prevent unauthorized access and mitigate the inherent risks posed by potential threats.
  
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


### My Steps to Harden Azure Virtual Machines

These are the steps I took to harden my machines to achieve the desired results. Please exercise caution, as certain settings can be irreversible. Note that I experimented with firewall settings, including excluding all inbound connections, to observe their effects. I reiterate that it's not advisable to exclude all inbound traffic as this will prevent you from entering the machine.

- **Delete Attack VM:**
  - Deleted the Attack VM to focus on securing Linux and Windows VMs only.
  - Some security suggestions may include settings for the Attack VM.

- **Network Configuration:**
  - Navigate to the Network tab.
  - Enable public network access for selected virtual networks and IP addresses only.

- **Windows VM Security:**
  - Access Windows VM settings.
  - Click on Extensions and Applications.
  - Add Microsoft Antimalware extension.
  - Connect to Windows via Remote Desktop.

- **Firewall Configuration:**
  - Adjust firewall settings.
  - Exclude all inbound connections.
  - **Note:** Experimented with this setting to observe its impact. This will prevent remote desktop access.
  - **Caution:** I reiterate that it's not advisable to exclude all inbound traffic.

- **Subnet Creation:**
  - Navigate to Virtual Networks.
  - Go to Settings and click on Subnets.
  - Add 2 subnets for Linux and Windows VM configurations.

- **Disk Encryption:**
  - [Learn more about Disk Encryption](https://learn.microsoft.com/en-us/azure/virtual-machines/disks-enable-host-based-encryption-portal?tabs=azure-powershell&WT.mc_id=Portal-Microsoft_Azure_Security#set-up-your-disk-encryption-set)

- **Exempted Hardening Processes:**
  - Some processes, such as installing Log Analytics agent on virtual machines, were exempted due to specific VM requirements.

- **Enable Backup:**
  - [Learn more about Azure Backup](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-monitor-and-troubleshoot#monitor-in-the-dashboard)

## Reflection
This project was both challenging and insightful, offering a glimpse into the daily operations of a Security Operations Center (SOC). Witnessing my work come together despite encountering errors was incredibly rewarding. The project highlighted the significant issues that can arise in an insured environment if it is not secured promptly. One of the most enlightening aspects was observing the state of an insecure system after 24 hours and then seeing the results after securing it, which underscored the importance of timely security measures. Throughout the project, I identified the locations of attackers, with some surprising results. The logs provided extensive information about the attackers, including their IP addresses and actions. This experience helped me understand the critical role of a SOC analyst in securing vulnerabilities before thousands of potential attackers can exploit them.

## Conclusion
This project involves creating a mini honeynet within the Azure platform, utilizing Microsoft Sentinel to create alerts and incident logs through seamless integration of various log sources from Microsoft Log Analytics Workspace. The Log Analytics Workspace effectively presents the desired metrics in a comprehensive manner, allowing for the measurement of the environment's vulnerabilities before hardening it. By fortifying the environment's infrastructure, I achieved a more desirable outcome—a reduction in security events and incidents, demonstrating that the environment was properly secured. This experiment included blocking all incoming traffic, and I may conduct further tests to examine the results of different hardening tactics. Results from any further tests will be posted in this repository.

It's worth noting that if the network's resources had been heavily used by regular users, it's possible that a higher volume of security events and alerts would have been generated within the 24-hour period after implementing the security controls.

## KQL Queries

| Metric                                       | Query                                                                                                                                            |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Start/Stop Time                              | range x from 1 to 1 step 1<br>\| project StartTime = ago(24h), StopTime = now()                                                                  |
| Security Events (Windows VMs)                | SecurityEvent<br>\| where TimeGenerated>= ago(24h)<br>\| count                                                                                   |
| Syslog (Linux VMs)                           | Syslog<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                                         |
| SecurityAlert (Microsoft Defender for Cloud) | SecurityAlert<br>\| where DisplayName !startswith "CUSTOM" and DisplayName !startswith "TEST"<br>\| where TimeGenerated >= ago(24h)<br>\| count |
| Security Incident (Sentinel Incidents)       | SecurityIncident<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                               |
| NSG Inbound Malicious Flows Allowed          | AzureNetworkAnalytics_CL<br>\| where FlowType_s == "MaliciousFlow" and AllowedInFlows_d > 0<br>\| where TimeGenerated >= ago(24h)<br>\| count    |


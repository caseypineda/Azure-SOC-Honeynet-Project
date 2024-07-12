![Linux](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/7287a240-c321-4aaf-a340-c8e68ad81643)
![Windows](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/717156d7-4026-49b0-8a7a-07f5d365f019)
![NSG](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/98d162d8-91d4-4be5-a69e-32c0075e5c58)
![MSSQL](https://github.com/caseypineda/Azure-SOC-Honeynet-Project/assets/136929096/042c2314-ae4a-4de7-93c3-e481328847c4)


`Start Time:` 2024-07-10T22:51:34 <br/>
`Stop Time:` 2024-07-11T22:51:34

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 6785
| SecurityAlert            | 3
| SecurityIncident         | 0 ->1
| AzureNetworkAnalytics_CL | 1832

**Note:** I'm unsure why security events and incidents aren't being counted. I may have missed a step or two when creating the pathways to these logs. This means I'll need to go through the entire process again in the future to better understand how to accurately connect the logs and obtain the intended data.
I may have found out what I may have done wrong make sure that you import Sentinel Analytics Rules before letting the machines rules for 24 hours.
The hardening techniques that I used is just:
- Closing the vulnerable ports
- Choosing the recommended block setting
- Adding Mircosoft Antimalware for Windows extension on the Windows VM


`Start Time:` 2024-06-27T22:51:34 <br/>
`Stop Time:` 2024-06-28T22:51:34


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 240
| SecurityAlert            | 0
| SecurityIncident         | 10
| AzureNetworkAnalytics_CL | 98

## Change after Securing Environment
| Metric                                          | Percent
| ----------------------------------------------- | -----
| Security Event (Windows VM)                     |  0.00%
| Syslog (Linux VM)                               | -96.46% 
| Security Alert (Microsoft Defender for Cloud)   | -100.00% 
| Security Incident (Sentinel Incidents)          |  900.00%
| NSG Inbound Malicious Flows Allowed             | -94.65%


Reflection

Conclusion


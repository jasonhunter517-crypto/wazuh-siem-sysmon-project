# Building and Testing a Wazuh SIEM with Sysmon

## Introduction
- **Detecting the Details: My Wazuh SIEM Deployment Project**


In this project, I improved a Wazuh SIEM deployment by tuning Sysmon logging on a Windows endpoint and testing how well the environment captured process activity, network connections, and behavior related to MITRE ATT&CK techniques. I also used Wazuh Discover and the MITRE ATT&CK dashboard to investigate the events generated during my experiments.

My name is Jason Hunter, and I am building my skills in cybersecurity through hands-on projects involving SIEMs, endpoint monitoring, log analysis, and threat detection. This project gave me the opportunity to work with Wazuh and Sysmon in a realistic lab environment and practice the same types of investigation and documentation skills that are important in cybersecurity roles. I am especially interested in learning how security tools can turn raw endpoint activity into useful information that analysts can use to detect and investigate suspicious behavior.

## Setup

For my SIEM modification, I updated the Sysmon configuration on the Windows endpoint `ad01` to improve visibility into process and network activity being forwarded to Wazuh. Before making the change, I reviewed the existing Sysmon configuration and confirmed that `ad01` was already sending events to Wazuh.

I then updated the Sysmon configuration to improve logging for important activity such as process creation, process access, and network connections. After applying the updated configuration, I generated activity on `ad01` by opening applications and creating network connections.

To verify the modification, I first checked Windows Event Viewer under the Sysmon Operational log. I then opened Wazuh Discover, filtered the results for the `ad01` agent, and searched for matching Sysmon events.

The modification was successful when the Sysmon events generated on `ad01` appeared in Wazuh with information such as process names, command-line data, timestamps, IP addresses, ports, and other event details. This confirmed that Sysmon was recording the activity and that Wazuh was successfully collecting the telemetry.

One mistake I made during the validation process was assuming that the logging configuration might have failed when I could not immediately find an expected event in Wazuh. I checked Windows Event Viewer and confirmed that Sysmon had actually recorded the event. I then realized that the issue was related to the time range and filters I was using in Wazuh Discover. After adjusting the time range and filtering specifically for `ad01` and the appropriate Sysmon fields, I was able to find the event.

This taught me that I should verify the event on the endpoint before assuming that the SIEM or configuration is not working. It also showed me how important the correct time range and filters are during log analysis.

## Experiment Time

### Experiment #1: Process Activity

For my first experiment, I tested whether Sysmon and Wazuh could capture process activity on the `ad01` Windows endpoint. I launched applications such as Command Prompt and Notepad and then reviewed the Sysmon Operational log in Windows Event Viewer.

After generating the activity, I opened Wazuh Discover and filtered the logs for the `ad01` agent. I looked for process-related information such as the process name, command line, parent process, user, timestamp, and Sysmon Event ID.

The process activity was successfully recorded by Sysmon and appeared in Wazuh. The events contained useful process information that could be used during an investigation. This confirmed that Wazuh was receiving process telemetry from `ad01`.

<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-10-51-003Z" src="https://github.com/user-attachments/assets/843bc548-1f9d-4f80-8f04-9c7c915f35b7" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-10-33-563Z" src="https://github.com/user-attachments/assets/1ce378f1-6a6c-4c04-ba93-734ccfff9d1a" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-10-06-912Z" src="https://github.com/user-attachments/assets/563a4159-6d9c-4fdd-950d-5e6182fb5782" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-09-15-202Z" src="https://github.com/user-attachments/assets/fe56ac36-af47-44d8-81aa-3ff6877985cc" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-07-41-645Z" src="https://github.com/user-attachments/assets/57700110-4254-4a88-a1f2-42b6983a7158" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-07-27-597Z" src="https://github.com/user-attachments/assets/f8b19e2c-2b16-46b3-aae7-9a16733e5f8a" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-06-59-123Z" src="https://github.com/user-attachments/assets/761b300d-b9f2-47ac-b4cb-9c97c25a75a7" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T15-06-51-020Z" src="https://github.com/user-attachments/assets/042ddd7d-3faa-4a1b-8131-4963b70de08f" />
**What I did:** I generated process activity on `ad01` by opening Command Prompt, running `whoami`, and launching Notepad.

**How I did it:** I performed the activity directly on the monitored Windows endpoint, then reviewed the Sysmon Operational log in Windows Event Viewer and searched Wazuh Discover for events from `ad01`.

**Expected result:** I expected Sysmon Event ID 1 to record the new process and expected the event to be forwarded to Wazuh with process name, command-line, parent process, user, and timestamp information.Sysmon successfully captured the process activity generated on ad01. The evidence showed a Sysmon Event ID 1 for process creation, including the process name, command-line information, parent process, user, and timestamp. The corresponding event also appeared in Wazuh, confirming that process telemetry from ad01 was successfully forwarded to the SIEM.

### Experiment #2: Network Activity

For my second experiment, I tested whether Sysmon and Wazuh could capture network connection activity from `ad01`. I generated a network connection from the Windows endpoint to another system or service in the lab environment.

I then reviewed the Sysmon logs and Wazuh Discover for network-related activity. I focused on information such as the source IP address, destination IP address, source and destination ports, protocol, and the process responsible for the connection.

Network-related events were visible in Wazuh and contained information about the connection, including IP addresses, ports, protocol, and process details. This showed that the SIEM could provide useful network telemetry for investigating activity from the endpoint.
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-20T06-08-35-875Z" src="https://github.com/user-attachments/assets/2d5a9702-476c-49c1-a079-5411e92de624" />
<img width="1136" height="495" alt="cs-screenshot-ad01-2026-08-21T21-33-48-331Z" src="https://github.com/user-attachments/assets/28fc5f4c-96d7-4273-bc1f-1fff47e82d90" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T21-35-20-427Z" src="https://github.com/user-attachments/assets/b0d4e8e4-3820-46bd-9db7-4c83791ddba0" />
<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T21-35-37-679Z" src="https://github.com/user-attachments/assets/2eb2905d-8a8c-417e-8828-c1ca412c298f" />

<img width="1136" height="520" alt="cs-screenshot-ad01-2026-08-21T21-35-50-817Z" src="https://github.com/user-attachments/assets/c4195992-2378-4899-9b31-3dc6f1f43700" />

**What I did:**  
I generated network activity from `ad01` by using Command Prompt to connect to another host.

**How I did it:**  
I ran a network command on `ad01`, then reviewed the Sysmon Operational log in Event Viewer and filtered Wazuh Discover for events from the `ad01` agent.

**Expected result:**  
I expected Sysmon Event ID 3 to record the connection and Wazuh to display the same network activity with source/destination IPs, ports, protocol, and process information.Sysmon successfully recorded the network connection generated from ad01. The event contained the source IP address, destination IP address, source/destination port information, protocol, and the process associated with the connection. The matching network event was also visible in Wazuh, confirming that network connection telemetry was being collected and forwarded correctly.
### Experiment #3: MITRE ATT&CK T1105

For my third experiment, I tested activity related to MITRE ATT&CK technique T1105, Ingress Tool Transfer, using Atomic Red Team in the lab environment.

After running the test, I searched Wazuh Discover for events generated by the activity and reviewed the Wazuh MITRE ATT&CK dashboard. I looked for process and transfer-related telemetry that could be connected to the test.

Wazuh recorded events generated during the experiment and provided process and activity information that could be used to investigate the simulated behavior. The MITRE ATT&CK dashboard also provided context for activity related to T1105, showing how SIEM telemetry can be connected to known attacker techniques.

During one of the experiments, I initially could not find the event I expected in Wazuh. At first, I thought the experiment or logging configuration had failed. I checked Windows Event Viewer and confirmed that Sysmon had recorded the activity. I then realized that the issue was caused by my Wazuh search settings. After adjusting the time range and filtering specifically for the `ad01` agent and relevant Sysmon fields, I was able to locate the event.

This reinforced the importance of confirming endpoint activity first and then checking the SIEM search configuration before assuming that data collection has failed.
<img width="1136" height="520" alt="cs-screenshot-ART_Workstation-2026-08-20T06-30-28-989Z" src="https://github.com/user-attachments/assets/87faf509-d625-4fbe-83d7-3454ce893d83" />
<img width="1136" height="520" alt="cs-screenshot-ART_Workstation-2026-08-20T06-36-06-935Z" src="https://github.com/user-attachments/assets/0968ec2c-6cd0-4fac-bcfc-7e9d5ea79dbe" />
<img width="1136" height="520" alt="cs-screenshot-Blue-Team_Workstation-2026-08-20T05-12-03-988Z" src="https://github.com/user-attachments/assets/036c113f-280d-42d6-a472-81ba226e362b" />
<img width="1136" height="520" alt="cs-screenshot-Blue-Team_Workstation-2026-08-20T05-12-16-311Z" src="https://github.com/user-attachments/assets/1e519dab-4c14-4f7f-a91a-c7145611a3a9" />
<img width="1136" height="520" alt="cs-screenshot-Blue-Team_Workstation-2026-08-20T06-40-33-976Z" src="https://github.com/user-attachments/assets/b4713244-694e-4753-9656-8a5605e71891" />
<img width="1136" height="520" alt="cs-screenshot-Blue-Team_Workstation-2026-08-20T06-40-48-116Z" src="https://github.com/user-attachments/assets/9047974b-24b0-437a-9221-d73f5f138989" />
<img width="1136" height="520" alt="cs-screenshot-Blue-Team_Workstation-2026-08-20T06-44-46-767Z" src="https://github.com/user-attachments/assets/19030b60-f550-41b8-a408-5c5d7441c60f" />
<img width="1136" height="520" alt="cs-screenshot-Blue-Team_Workstation-2026-08-20T06-45-01-562Z" src="https://github.com/user-attachments/assets/f8278878-2176-468f-aba7-f8a313e5ebbd" />
<img width="1136" height="520" alt="cs-screenshot-ART_Workstation-2026-08-20T06-35-54-936Z" src="https://github.com/user-attachments/assets/b6ebe09f-2a63-410e-b6f8-ba89150da518" />


**What I did:**  
I used Atomic Red Team to simulate MITRE ATT&CK technique T1105, Ingress Tool Transfer, in the lab environment.

**How I did it:**  
I executed the T1105 Atomic Red Team test from the ART Workstation, then reviewed the resulting events in Wazuh Discover and checked the Wazuh MITRE ATT&CK dashboard.

**Expected result:**  
I expected the test to generate process, network, or file-transfer telemetry that Wazuh could collect and use to investigate activity associated with T1105.The Atomic Red Team T1105 PowerShell Download test executed successfully. Wazuh captured activity generated by the PowerShell process during the test, providing evidence of the simulated ingress tool transfer behavior. The MITRE ATT&CK dashboard was then used to review the activity in the context of T1105.


## Conclusion

Overall, the experiments showed that the Wazuh SIEM was able to collect and display useful Sysmon telemetry from the `ad01` Windows endpoint. The first experiment confirmed that process activity could be recorded and investigated using fields such as process name, command line, parent process, user, timestamp, and Sysmon Event ID. The second experiment showed that network activity could also be reviewed in Wazuh using details such as source and destination IP addresses, ports, protocol, and the process responsible for the connection. The third experiment demonstrated how activity related to MITRE ATT&CK T1105, Ingress Tool Transfer, could be investigated using Wazuh Discover and the MITRE ATT&CK dashboard.

Together, these tests showed that improving Sysmon logging increased the amount of useful endpoint telemetry available in Wazuh and made it easier to investigate both normal and potentially suspicious activity.

One of the most important lessons from this project was not to assume that a missing event means the SIEM or configuration has failed. If an event does not appear in Wazuh, I would first verify that the event exists locally in Windows Event Viewer. After confirming that Sysmon recorded it, I would check the Wazuh time range, agent filter, event fields, and other search settings before changing the configuration.

I would also recommend making one configuration change at a time and documenting each change. This makes troubleshooting more organized and helps prevent unnecessary configuration changes when the actual problem may only be the way the logs are being searched.

## Final Thoughts

### The Coolest Thing I Learned

The coolest thing I learned during this project was how endpoint activity can be turned into useful security information inside a SIEM. Seeing Sysmon events from `ad01` appear in Wazuh made it easier to understand how analysts can trace process activity, network connections, and behavior associated with MITRE ATT&CK techniques. It helped connect the technical actions I was performing in the lab with the type of evidence a security analyst would actually investigate.

### One Piece of Advice

My advice to someone completing a similar project would be to verify each step before moving on. If an event does not appear in Wazuh, first check Windows Event Viewer to confirm that Sysmon recorded it. Then check the time range, filters, agent name, and event fields in Wazuh. This makes troubleshooting much easier and prevents you from changing a configuration that may already be working correctly.

### My Favorite Resource

My favorite resource during this project was the Wazuh documentation because it helped me understand how endpoint events are collected, searched, and analyzed inside the SIEM. I also found the MITRE ATT&CK documentation useful because it explained techniques such as T1105 and helped connect the lab activity to real-world attacker behavior.

### Thank You
References 
I would like to thank my TripleTen instructors and tutors for providing guidance throughout the project and helping me understand how to approach SIEM deployment and testing.
### 1. Sysmon

**Title:** Sysmon – Sysinternals  
**Author:** Mark Russinovich and Thomas Garnier / Microsoft Sysinternals  
**Affiliation:** Microsoft  
**How I used it:** I used the Sysmon documentation to understand process creation, network connection, and other Windows telemetry generated by Sysmon.
[Sysmon - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
### 2. Wazuh Log Collection

**Title:** Configuring Log Collection for Different Operating Systems  
**Author:** Wazuh Documentation Team  
**Affiliation:** Wazuh  
**How I used it:** I used this resource to understand how Wazuh collects Windows event logs and how endpoint logs can be forwarded to the SIEM.
[Configuring log collection for different operating systems](https://documentation.wazuh.com/current/user-manual/capabilities/log-data-collection/configuration.html)
### 3. Wazuh Log Analysis

**Title:** Log Data Analysis – Use Cases  
**Author:** Wazuh Documentation Team  
**Affiliation:** Wazuh  
**How I used it:** I used this resource to better understand how Windows endpoint events can be collected, searched, and analyzed in Wazuh.
[Log data analysis - Use cases · Wazuh documentation](https://documentation.wazuh.com/current/getting-started/use-cases/log-analysis.html)
### 4. MITRE ATT&CK T1105

**Title:** Ingress Tool Transfer – Technique T1105  
**Author:** MITRE ATT&CK  
**Affiliation:** The MITRE Corporation  
**How I used it:** I used this reference to understand T1105 and how adversaries can transfer tools or files into a compromised environment.
[Ingress Tool Transfer, Technique T1105 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1105/)

### 5. Atomic Red Team T1105

**Title:** Atomic Red Team – T1105 Ingress Tool Transfer Tests  
**Author:** Red Canary  
**Affiliation:** Red Canary / Atomic Red Team  
**How I used it:** I used this resource to understand how Atomic Red Team can simulate activity associated with MITRE ATT&CK T1105 for detection testing.
[redcanaryco/atomic-red-team: Small and highly portable detection tests based on MITRE's ATT&CK.](https://github.com/redcanaryco/atomic-red-team)

I would also like to thank the cybersecurity community and the developers behind tools such as Wazuh, Sysmon, MITRE ATT&CK, and Atomic Red Team for creating resources that make it possible to practice realistic security monitoring and detection techniques in a lab environment.

\# Web Server Breach Investigation & MITRE ATT&CK Mapping\
\
A cybersecurity project covering Android application security analysis, network traffic inspection, incident investigation, and MITRE ATT&CK mapping.\
\
\## Overview\
\
This project contains two practical exercises completed for the Vulnerability Assessment and Reverse Engineering lab:\
\
1\. \*\*Android APK Analysis:\*\* Examining the Signal Android application using MobSF, an Android emulator, PCAPdroid, and Wireshark. The objective was to compare static-analysis findings with observed runtime network activity and distinguish potential security concerns from evidence of malicious behavior.\
\
2\. \*\*Premium House Lights Incident Analysis:\*\* Investigating a simulated security breach using network diagrams, packet captures, access logs, and shell/database evidence. The attack activity was mapped to the MITRE ATT&CK framework.\
\
\## Lab 1: Android Application Analysis\
\
\### Tools and Environment\
\
\- Docker\
\- Mobile Security Framework (MobSF)\
\- Android Emulator\
\- Signal APK\
\- PCAPdroid\
\- Wireshark\
\
\### Methodology\
\
\*\*1. Environment Preparation\*\*\
\
MobSF was deployed using Docker. An Android emulator was prepared, the Signal APK was selected for examination, and Wireshark was used for network traffic analysis.\
\
\*\*2. Static Analysis with MobSF\*\*\
\
The APK was uploaded to MobSF and examined for:\
\
\- Application information and file hashes\
\- Requested and potentially dangerous permissions\
\- Hardcoded strings and possible secrets\
\- Android manifest configuration\
\- Application behavior and code-level findings\
\- Signing certificate information\
\
The static scan reported security-relevant items, including sensitive permissions, possible hardcoded secrets, manifest findings, and certificate-related warnings. These findings indicate areas requiring review; they do not, by themselves, prove that the application performed malicious actions.\
\
\*\*3. Runtime Traffic Capture\*\*\
\
PCAPdroid was installed in the Android emulator and used to capture application network traffic. The resulting capture was opened in Wireshark and inspected using HTTP and DNS-related filters.\
\
\*\*4. Static and Dynamic Findings Correlation\*\*\
\
The observed DNS requests were primarily associated with familiar services, including Google-related domains and \`comsats.edu.pk\`. The report did not identify suspicious domains, apparent DNS tunneling, repeated beaconing, or clear command-and-control traffic in the captured session.\
\
The comparison highlighted an important distinction: static-analysis warnings identify potential risks in an application, while runtime capture shows what was observed during the particular test session.\
\
\## Lab 2: Premium House Lights Security Breach\
\
\### Network Environment\
\
The incident scenario describes a network containing:\
\
\- \*\*Production VLAN:\*\* \`10.10.1.0/24\`, including the web server (\`10.10.1.2\`), database server (\`10.10.1.3\`), and file server.\
\- \*\*Employees VLAN:\*\* \`10.10.5.0/24\`, containing employee workstations and Wi-Fi connectivity.\
\- Firewall, switches, internet gateway, and Wi-Fi access point.\
\
\### Incident Summary\
\
The evidence examined in the report describes the following attack sequence:\
\
1\. \*\*Reconnaissance:\*\* An external IP address performed repeated requests against non-existent web paths and probed the exposed web server.\
2\. \*\*Initial Access:\*\* The attacker accessed \`/upload.php\`, an upload endpoint that did not adequately validate uploaded files.\
3\. \*\*Web Shell Deployment:\*\* A request to \`/uploads/shell.php\` indicated deployment or execution of a web shell.\
4\. \*\*Host Reconnaissance:\*\* Shell activity included commands such as \`netstat\`, \`sudo -l\`, and network scanning with Nmap.\
5\. \*\*Lateral Movement:\*\* The attacker used Telnet to access another internal host.\
6\. \*\*Database Access:\*\* The attacker accessed MySQL and enumerated databases and tables.\
7\. \*\*Collection and Exfiltration:\*\* A database dump was created with \`mysqldump\` and transferred using SCP to \`178.62.228.28\`.\
8\. \*\*Evidence Cleanup:\*\* The local dump file was deleted.\
\
The report identifies insufficient file-upload validation and permissive network access between segments as important weaknesses. In particular, traffic from the web-server segment to the database segment on MySQL port \`3306\` allowed the attacker to reach the database.\
\
\### MITRE ATT&CK Mapping\
\
\| Activity | MITRE ATT&CK Technique |\
\|---|---|\
\| Active Scanning | T1595.002 |\
\| Exploitation of a Public-Facing Application | T1190 |\
\| Web Shell Deployment | T1505.003 |\
\| Shell Command Execution | T1059.004 |\
\| System Information Discovery | T1082 |\
\| Python Interpreter Use | T1059.006 |\
\| File and Directory Discovery | T1083 |\
\| System Software Discovery | T1518 |\
\| Network Configuration Discovery | T1016 |\
\| Service Scanning | T1046 |\
\| Telnet for Remote Services | T1021.005 |\
\| Sudo Abuse | T1548.003 |\
\| Data from a Database | T1213 |\
\| Exfiltration over an Alternative Protocol | T1048 |\
\| File Deletion | T1070.004 |\
\
\## Key Findings\
\
\- Static APK analysis identified permissions, code, manifest, secret, and certificate items that warranted further examination.\
\- The captured Signal application traffic did not show clear command-and-control activity or suspicious DNS behavior during the observed session.\
\- The Premium House Lights incident involved an exposed upload endpoint and web shell deployment.\
\- Network segmentation was undermined by permissive communication between the web and database systems.\
\- Database access, data dumping, SCP transfer, and deletion of the local dump were documented in the incident evidence.\
\
\## Security Recommendations\
\
1\. Validate uploaded files and secure public-facing web applications.\
2\. Remove unnecessary test files and regularly scan web applications.\
3\. Apply least-privilege access to users and database accounts.\
4\. Restrict inter-VLAN traffic to explicitly required services.\
5\. Block unapproved file transfers from internal systems to the internet.\
6\. Log and review \`sudo\` activity and alert on unusual use.\
7\. Maintain an incident-response plan covering alerting, evidence collection, and impact assessment.\
8\. Provide regular security awareness training and review access rights periodically.\
\
\## Project Scope and Limitations\
\
\- The APK findings relate to the specific application sample and test environment used in the lab.\
\- Network conclusions are limited to the traffic captured during the observed session.\
\- The Premium House Lights investigation is based on the logs, packet captures, and other evidence described in the lab report.\
\- A static-analysis warning or a suspicious configuration should be treated as an indicator for investigation, not automatic proof of exploitation.\
\
\## Authors\
\
\- \*\*Farwah Hamid\*\* — FA22-BCT-007\
\- \*\*Manahil Khan\*\* — FA22-BCT-013\
\
\*\*Course:\*\* Vulnerability Assessment and Reverse Engineering  \
\*\*Submission:\*\* Lab Final

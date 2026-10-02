# Nmap Scan Report

Incident/Ticket ID: 1

Alert/Detection: ET SCAN Nmap Scripting Engine User-Agent Detected

Date/Time: 9/14/2025 1:05PM

Severity: Medium

Affected Host: Windows Server 2022 Virtual Machine

Affected User: N/A

Source IP: 192.168.0.22 Kali Linux Machine

Detection Source: Security Onion / Suricata

Initial Alert: ET SCAN Nmap Scripting Engine User-Agent Detected

Investigation: A simulated Nmap scan was performed from a Kali Linux virtual machine against a windows server 2022 virtual machine using Nmap with OS detection, service/version detection, default NSE scripts, and traceroute functionality. The Nmap scan showed ports like Kerberos on port 88 and LDAP port 389 open, and other services associated with the windows server 2022 virtual machine. Also did a drill down on the generated security event to see the source IP and the destination IP address. The destination source was a 2022 virtual machine and the source IP address was a Kali Linux machine due to the scanning capabilities.

Raw Event Evidence: ET Scan Nmap Scipting Engine User - Agent Detected

IOC Analysis: IoC indicators was the initial Nmap source IP address after doing a drill down on the security event generate and found out it was targeting the Windows Server 2022 destination IP address, the Nmap generated network traffic generated.

MITRE ATT&CK Mapping: Mapped to T1064 - Network Service Scanning, under the Discovery section. The activity consisted of scanning a remote host to see what services and operating service can be discovered.

Scope/Impact: The activity was limited to a nmap scan against the Windows Server 2022 virtual machine. The primary risk was the network service discovery that can provide information to adversaries to do further attacks like exploitation and credential attacks.

Containment: The host can be isolated from the network and the network traffic from the Kali Linux machine can be investigated and blocked

Remediation: Review exposed network services, close unnecessary service or ports, monitor for more Nmap scans on other machines, and maintain SIEM and IDS rules of detecting an Nmap scan. Create a rule in Suricata rules to identify the HTTP-agent associated with the Nmap Scripting Engine.

Escalation: Yes escalate to incident response team

Final Disposition: Simulated Nmap scan was detected successfully by Security Onion 2. A custom Kibana detection rule and a Suricata rule was created to improve detection of subsequent Nmap activity

Analyst Notes: The  Nmap scan on the windows server 2022 machine showed how security events and alerts can be generated and investigated. The custom Suricata rule was created to identify the HTTP user-agent associated with the Nmap Scripting Engine  

Lessons Learned / Detection Improvement: Creating rules in Kibana and in Suricata can help detect Nmap scans from unknown sources or IP addressses.


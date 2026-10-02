# Nmap Scan Report

Incident/Ticket ID: 1

Alert/Detection: ET SCAN Nmap Scripting Engine User-Agent Detected

Date/Time: 9/14/2025 1:05PM

Severity: Medium

Affected Host: Windows Server 2022 Virtual Machine

Affected User: N/A

Source IP: 192.168.0.22 Kali Linux Machine

Detection Source: Security Onion security event was generated

Initial Alert: ET SCAN Nmap Scripting Engine User-Agent Detected

Investigation: A kali Linux virtual machine was used to do an nmap scan against a windows server 2022 virtual machine. The nmap scan consisted of a OS detection, service/version, default NSE scripts, and a traceroute. All of these are capabilties of the Nmap scanning scripts. The Nmap scan showed multiple ports that where open, some of the ports open where Kerberos on port 88 and LDAP on port 389 open. I also did a drill down on the generated security event to see the IP source and the destination IP address. The destination IP address was a 2022 virtual machine and the IP source address was a Kali Linux machine.

Raw Event Evidence: ET Scan Nmap Scripting Engine User - Agent Detected was generated from Security Onion 2

IOC Analysis: After a drill down was performed on the security event generated on Security Onion 2, the source IP address and the destination IP address was discovered. The source IP address was target the windows server 2022 virtual machine and based on the security event created by Security Onion the source IP address was a Kali Linux machine.

MITRE ATT&CK Mapping: Mapped to T1064 - Network Service Scanning, under the Discovery section and also T1059 Command and Scripting Interpreter because of the default NSE scripts being used to do a OS detection and service/version detection. 

Scope/Impact: An nmap scan was done against the windows server 2022 virtual machine. The main concern was the service discovery that can provide more information to bad actors to continue an attacks like exploitation or credential attacks.

Containment: The host can be isolated from the network and the network traffic from the Kali Linux machine can be investigated and blocked by creating a rule in Kibana or Suricata.

Remediation: Find exposed network services by doing an internal Nmap scan, close unnecessary service or ports, and monitor for more Nmap scans on other machines. Createc a rule in Suricata rules to identify the HTTP-agent associated with the Nmap Scripting Engine.

Here is an example of the Suricata rule that can be created to detect future scans:
alert http any -> $HOME_NET any (msg: "Kali Suricata Nmap Alert"; flow:established,to_server; http.user_agent;content:"Mozilla/5.0 (compatible |3b| Nmap Scripting Engine"; nocase; startswith; classtype:web-application-attack; sid:; rev:;)

Escalation: Yes escalate to incident response team

Final Disposition: An Nmap scan was detected by Security Onion 2. A custom Kibana detection rule and a Suricata rule was created to detect future Nmap scans and to detect the NSE scripts found in Nmap.

Analyst Notes: An Nmap scan on the windows server 2022 generated reports on Security Onion 2, after doing a drill down on the generated report, the source IP and the destination IP was discovered. A custom Suricata rule was created to identify the HTTP user-agent associated with the Nmap Scripting Engine to detect similar events.

Lessons Learned / Detection Improvement: Creating a rule in Suricata rule section can help detect Nmap scans in the future.

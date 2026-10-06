# Performing an Nmap scan on Windows Server 2022
- On security Onion 2 terminal, make a few folder with the name SOC
- Then type in this command: sudo tcpdump -ni ens192 'host <kali linux IP> and host <windows server IP>' -w /home/me/SOC/nmap-scan.pcap
- This will output all of the captured logs from the kali Linux scan for later use
- If you want to see what was captured, type in the following command: sudo tcpdump -nn -r /home/me/SOC/nmap-scan.pcap
- This will display the original captured network logs from the Nmap scan
- This command will show you the human readable details like size, permission and other things: ls -lh /home/me/SOC/nmap-scan.pcap
- To see it with more details use this command: sudo tcpdump -nn -r /home/me/SOC/nmap-scan.pcap -c 20
- This command is to take a screenshot if you need to: gnome-screenshot -a
- If you want to inspect the network logs use this command: sudo tcpdump -nn -r /home/me/SOC/nmap-scan.pcap | less

# Security Onion 2
- Go back to Security Onion 2, click on Alerts
- You will see many different alerts after doing a nmap scan
- Lets start with ET SCAN Possible Nmap User-Agent Observed, click on the arrow icon, there is a ticket filled out on the important parts of the alert






# Ticket

Title: ET SCAN Possible Nmap User-Agent Observed

Timestamp: 2026-10-06T00:09:33.765Z

Detection: suricata

Destination: 192.168.0.24

Severity: High

network.data.decoded: 
- GET /HNAP1 HTTP/1.1
- Connection: close
- User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)
- Host: 192.168.0.24:5985

network.transport: TCP

rule: alert http $HOME_NET any -> any any (msg:"ET SCAN Possible Nmap User-Agent Observed"; flow:established,to_server; http.user_agent; content:"|20|Nmap"; fast_pattern; classtype:web-application-attack; sid:2024364; rev:5; metadata:affected_product Any, attack_target Client_and_Server, created_at 2017_06_08, deployment Perimeter, performance_impact Low, confidence Medium, signature_severity Informational, updated_at 2024_03_07, reviewed_at 2024_05_06;)

Source: 192.168.0.34

Triage:
There is strong evidence that Nmap's scripting Engine generated this alert, the scan used port 5985 which is used for Microsoft WinRM (Windows Remote Management) over HTTP, the activity in the PCAP shows a scan of multiple ports in a few seconds with the syn flag which indicates the first part of the 3 way handshake. This appears to be really suspicious as this is not expected or a planned scan. Also the IP address of the source is not a known ip address within the network. Suricata looked at the HTTP User-agent in teh network traffic and found it to be associated with the Nmap scripting engine.

Analysis:
After review of the PCAP logs with multiple port scan within a small window of time, the rogue IP address and the alert generated and nmap script engine being used, there is strong evidence of this being a simulated Nmap scan of the Domain controller.

Conclusion:
There is strong evidence of an simulated Nmap scan on the domain controller, the evidence consist of network logs showing ports being scanned in a small window and the alert generated showing the Nmap scripting engine being used. The activity is a True Positive for the Suricata detection

Escalation:
Escalated to tier 2 due to an unathorized and a unplanned Nmap scan on a domain controller. There where no plans or notice of that was going to happen.

Action:
The alert was investigated, a ticked was filled out with the necessary information and escalated to tier 2. 

Status: Open / Escalated / True Positive

Evidence:
[PCAP, alert ID, screenshots, logs, queries, etc.]

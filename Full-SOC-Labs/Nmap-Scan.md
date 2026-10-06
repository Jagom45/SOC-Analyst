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
- sudo tcpdump -nn -r /home/me/SOC/nmap-scan.pcap 'host 192.168.0.34 and host 192.168.0.24 and tcp' | grep 'Flags'
- sudo tcpdump -nn -r /home/me/SOC/nmap-scan.pcap 'host 192.168.0.34 and host 192.168.0.24 and tcp' | grep 'Flags' | grep '\[S\.'
- sudo tcpdump -nn -r /home/me/SOC/nmap-scan.pcap 'host 192.168.0.34 and host 192.168.0.24 and tcp port 999'
- sudo tcpdump -nn -A -r /home/me/SOC/nmap-scan.pcap 'host 192.168.0.34 and host 192.168.0.24 and tcp port 999'

# Security Onion 2
- Go back to Security Onion 2, click on Alerts
- You will see many different alerts after doing a nmap scan
- Lets start with ET SCAN Possible Nmap User-Agent Observed, click on the arrow icon, there is a ticket filled out on the important parts of the alert at the bottom

# Cases
- Go to the cases section of security Onion 2
- Click on the blue + button
- Give it a title and a description and paste the ticket in the notes section
- Click on save

# Alerts
- Go back to alerts
- Click on the blue triangle to escalate the alert
- A pop up will show up, click on the name of the case that you created under Cases and add it there
- You will see a blue notification saying that escalating groups of alerts may take a while and will continue in the background
- The alert should be gone now

# Cases
- Go back to cases and click on the binocular icon to see if it worked, you will see your notes there and the information from alerts should migrated over
- Click on the link icon and use the ID to filter out and find the instance for that specific Nmap scam
- You will see other logs there that are correlated to the Nmap scan if you did multiple cans
- Go back to the cases main page
- Find the status field which should say new under the Summary section on the right and select closed
- Go back to the cases main page, the case should be gone now as it is now closed
- Click on the drop down arrow and select closed cases and you will find the case that you just closed there





# Ticket

- Title: ET SCAN Possible Nmap User-Agent Observed

- Timestamp: 2026-10-06T00:09:33.765Z

- Detection: suricata

- Destination: 192.168.0.24

- Severity: High

- network.data.decoded: 
  - GET /HNAP1 HTTP/1.1
  - Connection: close
  - User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)
  - Host: 192.168.0.24:5985

- network.transport: TCP

- rule: alert http $HOME_NET any -> any any (msg:"ET SCAN Possible Nmap User-Agent Observed"; flow:established,to_server; http.user_agent; content:"|20|Nmap"; fast_pattern; classtype:web-application-attack; sid:2024364; rev:5; metadata:affected_product Any, attack_target Client_and_Server, created_at 2017_06_08, deployment Perimeter, performance_impact Low, confidence Medium, signature_severity Informational, updated_at 2024_03_07, reviewed_at 2024_05_06;)

- Source: 192.168.0.34

- Triage: There is strong evidence that Nmap's scripting Engine generated this alert, the scan used port 5985 which is used for Microsoft WinRM (Windows Remote Management) over HTTP, the activity in the PCAP shows a scan of multiple ports in a few seconds with the syn flag which indicates the first part of the 3 way handshake. The IP address of the source is not a known ip address within the network. Suricata looked at the HTTP User-agent in the network traffic and found it to be associated with the Nmap scripting engine. The other alerts being generated and picked up different ports being scanned also is evidence of a nmap scan.

- Other alerts associated with the nmap scan:
- 8 ET SCAN Nmap Scripting Engine User-Agent Detected (Nmap Scripting Engine)	suricata	high	2009358
- 8	ET SCAN Possible Nmap User-Agent Observed	suricata	high	2024364
- 4	ET INFO GIOP/IIOP Request Outbound	suricata	high	2034730
- 4	ET INFO Outbound MSSQL Connection to Non-Standard Port - Likely Malware	suricata	medium	2013409
- 4	ET SCAN MS Terminal Server Traffic on Non-standard Port	suricata	medium	2023753
- 2	ET INFO RDP - Response To External Host	suricata	low	2001330
- 2	ET INFO RMI Request Outbound	suricata	high	2034718
- 2	ET SCAN Suspicious inbound to MSSQL port 1433	suricata	medium	2010935
- 2	ET SCAN Suspicious inbound to Oracle SQL port 1521	suricata	medium	2010936
- 2	ET SCAN Suspicious inbound to PostgreSQL port 5432	suricata	medium	2010939
- 2	ET SCAN Suspicious inbound to mySQL port 3306	suricata	medium	2010937
- 2	GPL DNS named version attempt	suricata	medium	2100257
- 1	ET INFO Observed Google DNS over HTTPS Domain (dns .google in TLS SNI)	suricata	low	2047866
- 1	ET SCAN Potential VNC Scan 5800-5820	suricata	medium	2002910
- 1	ET SCAN RDP Connection Attempt from Nmap	

- Analysis: After review of the PCAP logs with multiple port scan within a small window of time, the rogue IP address and the alert generated and nmap script engine being used, there is strong evidence of this being a simulated Nmap scan of the Domain controller.

- Conclusion: There is strong evidence of an simulated Nmap scan on the domain controller, the evidence consist of network logs showing ports being scanned in a small window and the alert generated showing the Nmap scripting engine being used. The activity is a True Positive for the Suricata detection as suricata correctly identifited Nmap traffic.

- Escalation: Escalated to tier 2 due to an unathorized and a unplanned Nmap scan on a domain controller. There where no plans or notice of that was going to happen.

- Action: The alert was investigated, a ticked was filled out with the necessary information and escalated to tier 2.

- Recommendation - Continue monitoring the domain controller for additional scanning activity. Since the scan was a simulated one no additional actions like containment or blocking is required. But in a real environment considering containment or blocking the ip address would be recommended.

- Status: Open / Escalated / True Positive

- Evidence:
  - Security Onion Suricata alert - SID 2024364
  - nmap-scan.pcap

- Note: One nmap scan causes multiple network probes, multiple suricata signatures, multiple alerts and it sometimes will be one SOC case

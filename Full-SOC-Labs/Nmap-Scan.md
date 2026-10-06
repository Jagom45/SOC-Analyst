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
- Lets start with ET SCAN Possible Nmap User-Agent Observed






# Ticket

Title: [Alert / Incident Name]

Severity: Low / Medium / High / Critical

Source: [Source IP / Host]

Destination: [Destination IP / Host]

Detection: [Security Onion / Suricata / Zeek / Elastic]

Summary:
[1–2 sentences describing what happened.]

Analysis:
[What you checked and what you found.]

Action:
[What was done — investigated, contained, blocked, escalated, or monitoring.]

Status: Open / Investigating / Resolved / False Positive

Evidence:
[PCAP, alert ID, screenshot, log, etc.]

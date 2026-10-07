# Performing a Hydra-Brute-Force Attack
- Go to Security Onion 2 terminal
- Then type in this command: sudo tcpdump -ni ens192 'host <kali linux IP> and host <windows server IP>' -w /home/me/SOC/hydra-rdp.pcap
- This will log the network attactivity from hyrda
- To look at the logs type in the following command:  tcpdump -nn -r /home/me/SOC/hydra-rdp.pcap



# Kali Linux
- On Kali linux type this command into the command prompt: hydra -t 1 -V -f -l sally -P rockyou.txt 192.168.0.24 rdp
- Here is what the command does:
- hydra	- Run the Hydra login-testing tool
- t 1	- Uses 1 concurrent connection/task
- V	- Verbose — Displays each login attempt
- f	- Stop after finding a valid login
- l sally	- Use the username sally
- P rockyou.txt	- Read passwords from rockyou.txt, this is also the path
- 192.168.0.24	- Target Windows machine
- rdp	- Use Remote Desktop Protocol
- This will create an alert on Security Onion 2

# Security Onion 2
- Go to alerts
- You will see: 8	ET INFO RDP - Response To External Host	suricata	low	2001330

# Security Onion 2
- Go back to Security Onion 2, click on Alerts
- You will see many different alerts after doing a nmap scan
- Lets start with ET INFO RDP - Response to External Host Suricata, click on the arrow icon, there is a ticket filled out on the important parts of the alert at the bottom

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

Title: ET INFO RDP - Response To External Host

rule.uuid - 2001330

Summary: This rule detects an RDP (Remote Desktop Protocol) response packet being sent from a host within the home network to any external host on the internet. The detection is based on specific byte patterns at particular positions in the packet, indicating the start of an RDP connection response.

Timestamp: 2026-10-06T22:54:49.602Z

Detection: suricata

Destination: 192.168.0.35

Port: 33178 

Severity: low

network.transport: TCP

rule.rule: alert tcp $HOME_NET 3389 -> $EXTERNAL_NET any (msg:"ET INFO RDP - Response To External Host"; flow:established,to_client; content:"|03|"; offset:0; depth:1; content:"|D0|"; offset:5; depth:1; classtype:misc-activity; sid:2001330; rev:10; metadata:attack_target Client_and_Server, created_at 2010_07_30, deployment Perimeter, performance_impact Significant, confidence Medium, signature_severity Informational, updated_at 2023_04_25, reviewed_at 2024_05_02; target:src_ip;)

Source: 192.168.0.24

Source.ip: 3389

Triage: I checked the source ip address and its using port 3389 which is Remote Desktop Protocol (RDP), the rule that detected triggered because an internal computer using port 3389 tried to communicate with an external computer on any port, there is evidence that an internal computer is trying communicate with a remote computer. Evidence from the network logs from hydra-rpd.pcap also shows strong evidence of an external host trying to communicate with an internal host 192.168.0.24:3389.

Analysis: After reviewing the source IP address, the port of the source IP address and the destination IP address and the destination port, there is enough evidence that an internal computer tried to communicate with an external host. The rule was triggered correctly. 192.168.0.24:3389 to 192.168.0.35:33178. This correlates the the hydra attack that we did previously.

Conclusion: This is an intent that an internal computer communicate to a remote computer. This alert is a True positive and will be escalated. Suricata correctly detected RDP response traffic with external host.

Escalation: Escalated to SOC 2

Action: The alert was investigated using the source IP address and the port being used along with the external IP address along with reading over the rule that was triggered. Incident was escalated to SOC 2 and marked as True Positive.

Recommendation: Isolate the domain controller or block 192.168.0.35 IP address to prevent further attempts from an internal computer to communicate with an external computer, and continued monitoring of internal computers trying to communicate on port 3389 with external hosts

Status: Open/Escalated/True Positive

Evidence:
hydra-rpd.pcap
PCAP shows evidence of RDP traffic from the kali linux machine to the windows system on port 3389.

Going to make a hydra detection rule later.


















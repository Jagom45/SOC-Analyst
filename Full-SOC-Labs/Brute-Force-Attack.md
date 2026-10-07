# Performing a Brute-Force Attack
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

Triage: I checked the source ip address and its using port 3389 which is Remote Desktop Protocol (RDP), the rule that detected triggered because an internal computer using port 3389 tried to communicate with an external computer on any port, there is evidence that an internal computer is trying communicate with a remote computer

Analysis: After reviewing the source IP address, the port of the source IP address and the destination IP address and the destination port, there is enough evidence that an internal computer tried to communicate with an external host. The rule was triggered correctly. 192.168.0.24:3389 to 192.168.0.35:33178. This correlates the the hydra attack that we did previously.

Conclusion: This is an intent that an internal computer communicate to a remote computer. This alert is a True positive and will be escalated. Suricata correctly detected RDP response traffic with external host.

Escalation: Escalated to SOC 2

Action: The alert was investigated using the source IP address and the port being used along with the external IP address along with reading over the rule that was triggered. Incident was escalated to SOC 2 and marked as True Positive.

Recommendation: Isolate the domain controller or block 192.168.0.35 IP address to prevent further attempts from an internal computer to communicate with an external computer, and continued monitoring of internal computers trying to communicate on port 3389 with external hosts

Status: Open/Escalated/True Positive

Evidence:
[PCAP, alert ID, screenshots, logs, queries, etc.]

Going to make a hydra detection rule later.


















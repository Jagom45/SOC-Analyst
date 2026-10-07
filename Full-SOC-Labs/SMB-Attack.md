* Performing a SMB enumeration
- On the Security Onion 2 terminal type in the following command: sudo tcpdump -ni ens192 'host 192.168.0.35 and host 192.168.0.24' -w /home/me/SOC/smb-enumeration.pcap

# Kali linux
- On Kali Linux type in the following command in the terminal: nmap -sV -p 445 192.168.0.24

# Security Onion 2
- Press ctrl+Z to stop the network captures on the terminal

# Security Onion 2
- You will see different alerts that are related to the nmap scan that was performed:
- 1	ET INFO GIOP/IIOP Request Outbound	suricata	high	2034730
- 1	ET INFO Outbound MSSQL Connection to Non-Standard Port - Likely Malware	suricata	medium	2013409
- 1	ET INFO RMI Request Outbound	suricata	high	2034718
- 1	ET SCAN MS Terminal Server Traffic on Non-standard Port	suricata	medium	2023753

- Open 1 ET INFO Outbound MSSQL Connection to Non-Standard Port - Likely Malware












# Ticket

Title: ET INFO Outbound MSSQL Connection to Non-Standard Port - Likely Malware

rule.uuid - 2013409

Summary: This rule detects an outbound MSSQL connection attempt from a protected network to an external network targeting a non-standard port, which may indicate potential malware activity. The rule examines TCP traffic for specific patterns associated with MSSQL communication, identifiable by certain byte sequences present in the traffic data. It flags such attempts as unusual and possibly malicious, given that valid MSSQL connections typically occur over standard ports.

Timestamp: 2026-10-07T02:37:22.958Z

Detection: suricata

Destination: 192.168.0.24

Port: 445

Severity: medium

network.transport: TCP 

rule.rule: alert tcp $HOME_NET any -> $EXTERNAL_NET !1433 (msg:"ET INFO Outbound MSSQL Connection to Non-Standard Port - Likely Malware"; flow:established,to_server; flowbits:set,ET.MSSQL; content:"|12 01 00|"; depth:3; content:"|00 00 00 00 00 00 15 00 06 01 00 1b 00 01 02 00 1c 00|"; distance:1; within:18; content:"|03 00|"; distance:1; within:2; content:"|00 04 ff 08 00 01 55 00 00 00|"; distance:1; within:10; classtype:bad-unknown; sid:2013409; rev:5; metadata:created_at 2011_08_16, confidence High, signature_severity Informational, tag Description_Generated_By_Proofpoint_Nexus, updated_at 2024_03_14;)

Source.ip: 192.168.0.35

Port: 40530

Triage: Reviewed the Suricata alert and identified the source being 192.168.0.35:40530 and the destination being 192.168.0.24.445. The destination port is TCP 445 which is normally used for SMB. The rule triggered detects MSSQL related traffic being sent from a non-standard port, instead of the typical MSSQL port 1433. A nmap scan was done to simulate a service/version detection. 

Analysis: An external computer tried to communicate with an internal IP address. The external IP address being 192.168.0.35:40530 to 192.168.0.24:445. Port 445 normally being SMB. This was detected as MSSQL related packet patterns. Packets where being sent to destination port 445, which is a non-standard destination port for MSSQL compared to the typical port of 1433

Conclusion: The alert is a True Positive, and will be escalated to SOC 2. All of the events are correlated to this one.

Escalation: Escalated to SOC 2

Action: The alert was investigated using the source IP, source port, destination IP, destination port, and Suricata rule details. The event was correlated to an unauthorized SMB enumeration activity originating from 192.168.0.35:40530 against the internal computer 192.168.0.24. It alert was escalated to SOC 2 for review. 

Recommendation: Continue monitoring for SMB enumeration activity and similar alerts and continue monitoring network logs as well.  

Status: Open/Escalated/True Positive

Evidence:
- SMB-enumeration.pcap
- Suricata SID 2013409
- Security Onion Alert


Rule:
tcp -	TCP
$HOME_NET -	192.168.0.35
any source port	- 40530
->	.35 → .24
$EXTERNAL_NET	- 192.168.0.24 as classified by the rule
!1433 - destination port	445 because 445 ≠ 1433




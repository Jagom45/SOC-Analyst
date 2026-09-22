# Simulated Intrustion & Kibana rule creation
- This section will show you how to perform a simluated intrustion and making a rule in Kibana to detect further nmap scans and creating a Suricata rule to drill down on the offender's IP

## Nmap Scan
- Turn on your kali linux, windows server 2022 and your Security Onion 2 vertirtual machine

- Go towards your Kali linux virtual machine and type in your username and your password
- Click on the terminal icon to open a terminal
- Type in sudo su and press enter, type in your password and press enter
- Enter the command namp -A <IP address of Windows Server 2022> (This can will do OS detection, version detection, run default NSE scriptsa and perform a traceroute), this will make a lot of noise and be detected by Security Onion 2
- This is classified as Network Service Discovery T1046, tactic: Discovery under MITRE Att&CK, you can also check out under the MITRE ATT&CK website the threat groups that do newtork discovery scans using Nmap.
- The nmap scan will show ports like 88 which is Kerberos, 389 which is AD LDAP and OS as Windows Server 2022
- On the Security Onion 2 home page, click on Alerts, you should see some alerts generated from the Nmap scan like ET SCAN Nmap Scripting Engine User-Agent Detected
- Click on one of the Alerts, you can do a drill down and see how many times that specific alert was generated and see the timestamps, rule name that triggered the alert generated, source ip, destination ip and much more

## Creating a SIEM Rule in Kibana to detect further Nmap scan from the kali linux virtual machine
- Click on the 3 bars and click on Kibana
- Type in your username and your password
- Click on the 3 bars and then under Security click on Rules, click on Detection rules (SIEM), click on Create new rule button, click on Threshold, in the Custom query type in source.ip:<Kali Linux IP address> AND event.dataset.keyboard:suricata.alert, click on the next button, Under the about rule, give it a name and description, click on the continue button, click on the continue button and then click on the Create & enable rule
- Run the nmap scan again, the rule should triggered and security events should be generated, click on security, then click on Alerts under Kibana
- Click on the 3 bars on Kibana, then click on Stack Management, click on Rules, click on the name rule that you just created, then click on history and you will see when the rule was executed succesfully

## Creating Suricata Alert
- Go back to the Security Onion 2 dashboard, click on the 3 bars, click on the Detections, click on the add button, under the Add Detection, select Suricata, for license click leave it blank, for signature do not delete the progenerated SID number, type the following in the signature text box:

alert http <Kali Linux Virtual Machine IP> any -> $HOME_NET any (msg: "Kali Suricata Nmap Alert";
flow:established,to_server; https.user_agent;content:"Mozilla/5.0 (compatible |3b| Nmap Scripting Engine"; nocase; startswith; classtype:web-application-attack; sid:<type in teh pregenerated sid number here>; rev:;)

- Then click on Create
- You have now created your first Suricata rule in Security Onion 2 that will detect the HTTP user agent associated with Nmap's scripting engine



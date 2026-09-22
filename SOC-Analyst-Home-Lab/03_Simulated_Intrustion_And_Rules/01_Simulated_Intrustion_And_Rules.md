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

## What the Kibana rule actually does
- source.ip: AND event.dataset.keyboard:suricata.alert
- source.ip - Show Suricata alert events where the source IP is that specific IP address
- AND - Both conditions must be true
- event.dataset:suricata.alert - Only look at events identified as Suricata alerts
- In short, show me Suricata alerts where the network traffic came from that specific ip address

## Creating Suricata Alert
- Go back to the Security Onion 2 dashboard, click on the 3 bars, click on the Detections, click on the add button, under the Add Detection, select Suricata, for license click leave it blank, for signature do not delete the progenerated SID number, type the following in the signature text box:

- alert http <Kali Linux Virtual Machine IP> any -> $HOME_NET any (msg: "Kali Suricata Nmap Alert";
flow:established,to_server; https.user_agent;content:"Mozilla/5.0 (compatible |3b| Nmap Scripting Engine"; nocase; startswith; classtype:web-application-attack; sid:<type in the pregenerated sid number here>; rev:;)

- Then click on Create
- You have now created your first Suricata rule in Security Onion 2 that will detect the HTTP user agent associated with Nmap's scripting engine

## What the rules actually does
 - http - This line looks for HTTP traffic
 - Kali Linux Virtual Machine IP - The traffic must come from this IP address
 - any - The source can use any port
 - (->) - The traffic is going from left to right
 - $HOME_NET - The destination must be an IP address in your defined home network
 - any - The destination can use any port
 
 - flow: Specifies characterisitcs of the network connection
 - established - Only look at an established TCP connection
 - to_server - Look at traffic going towards the server

 - http.user_agent - Look at the HTTP user-agent field
 - content:"Mozilla/5.0 (compatible |3b| Nmap Scripting Engine" - Search for this specific piece of text, Suricata will inspect the field rather than searching everywhere in the packet
 - |3b| - That's the hexadecimal notation, 3b in hexadecimal represents ;
 - nocase - Ignore capitalization when comparing the text
 - startswith - The specified content must appear at the beginning of the HTTP user-agent field, in this case (Mozilla/5.0 (compatible |3b| Nmap Scripting Engine)
 - In short this is what the rule is doing - Watch for HTTP traffic from this IP address to my home network, if the HTTP user agent starts with Mozilla/5.0 (compatible |3b| Nmap Scripting Engine, detect it regardless of capitalizaiton





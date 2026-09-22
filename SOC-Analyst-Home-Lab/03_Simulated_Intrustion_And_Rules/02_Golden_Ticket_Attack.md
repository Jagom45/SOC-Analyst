# Golden Ticket Attack
- This will show you how to perform a Golden Ticket Attack

## Configuring a vulnerable user in Windows Server 2022
- On Windows Server 2022, enable Remote Desktop Settings
- On CMD, type in net user sally password /add /domain (This will make their password as password)
- This will be classified as Common Weakness Enumeration 521 Weak Password Requirements
- Now make sure sally has remote access privileges by clicking on the start button, click System, then click on Remote Desktop, under User accounts click on Select users that can remotely access this PC, then click on sally, if sally is not there, click on add, then click on Advanced button, under Common Queries type in sally in the name text box, and then click on find, double click on her name and then click on ok
- Now add Allow logon through Remote Desktop Service by clicking on the search bar on the bottom left, type in gpedit.msc and then under Computer Configuration click on Windows Settings, Security Settings, Local Policies and then on User Rights Assignment, click on Allow logon through Remote Desktop Services, here you will edit the allowed users to be domain admins, local admins, IT admins and Sally by clicking on the Add Users or Group button
- You will do the same for Allow log on locally Policy under User Rights Assignment
- Do a right lick on Default Domain Policy on the Group Policy Management Window and click on Enforce

## Reconnaissance and Gaining Access with a remote brute force
- Perform another Nmap scan with the command Nmap -A <Kali Linux IP address>, the results should be similar from before, now also showing port 3389 for Remote Desktop Protocal
- Type in the command gunzip rockyou.txt.gz to unzip the rockyou.txt file if you haven't yet
- Now you will perform a remote brute force attack against sally account
- In a terminal type in hydra -t 1 -V -f -l sally -P /usr/share/wordlists/rockyou.txt <Windows Server 2022 IP address> rdp

# Incident Response
- This will  consist of doing a incident response, forensics and recovery

# Using Virustotal
- You can upload the Mimikatze.exe file from the downloads folder to Virus total and see the different hashes for mimispool.dll, mimilib.dll, mimikatze.exe, mimidrv.sys, and mimilove.exe

# Disconnecting from the newtork/VM network adaptoers
- You can also disconnect the affected host from the network
- Right click on the Vmware Workstation launcher, select the Windows Server 2022 virtual machine, click on the VM tab, click on Settings, click on the Network Adapters that are listed, click on the Remove button and then click on the OK button

# Modifying the host-based firewall to disconect from the network
- In a incident response scenario or in an emergency situation, you can modify the host's firewall with a quick PowerShell command to block all incoming and outgoing traffic to the malicious IP address
- Type in the following command in the affected hosts using PowerShell:
- New-NetFirewallRule -DisplayName "Block IP Inbound" -Direction Inbound -Protocol Any -Action Block -RemoteAddress <Kali Linux IP Address>
- New-NetFirewallRule -DisplayName "Block IP Outbound" -Direction Outbound -Protocol Any -Action Block -RemoteAddress <Kali Linux IP Address>
- Try using a cmd ping command, ping <Kali Linux IP Address>, it will fail

# Simulating a Ransomware Attack
- Before we simulate the Ransomware Attack, we will create a snapshot of the Windows Server 2022 virtual machine
- Click on the VM tab, click on Snapshot, take Snapshot, Under name give it a name and a description, and then click on the Take Snapshot button
- You have now created a snapshot of the Windows Server 2022 virtual machine, this will be used for recovery later on

## Downloading the ransomware
- Login back into your Windows Server 2022 virtual machine as sally
- Go to this link, https://github.com/NextronSystems/ransomware-simulator/releases
- Download the quickbuck.exe

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
- Go to this link: https://github.com/NextronSystems/ransomware-simulator/releases
- Download the quickbuck.exe file
- Go to the downloads folder and extract the files
- You will see notifications pop up on the right side of the screen
- Open up CMD or PowerShell and navigate to your Downloads folder and type in the following command: quickbuck.exe
- Once quickbuck.exe is done running, it will create a file named ransomware-simulator-note.txt on the desktop
- You can open up the file name and read the contents if you wish
- Go back to Kibana Alerts, you will see many alerts where generated because of the ransomware
- You will also see a vssadmin.exe command with the following arguments:
- vssadmin delete shadows /for=nonrealvolume /all /quiet
- This attack would fall under Inhibit System Recovery according to MITRE ATT&CK
- You can great a Kibana Rule to block quickbuck.exe from being downloaded by using the hash of the file by uploading the file to virustotal and grabbing the hash of the file and then creating a rule to block anything that has that same exact hash by using hash blocks

## Using FTK to perform forensics on Windows Server 2022
- Forensics Toolkit (FTK) is a software to collect evidence and analyze hosts
- Go to this link and download https://www.exterro.com/digital-forensics-software/ftk-imager on your Windows Server 2022 virtual machine under the user sally, unzip it and install the program
- We are going to use FTK to perform a memory capture and memory dump
- Turn off your Windows Server 2022 virtual machine
- On the VMware Workstation, click on the VM tab, select Settings tab, select Hard Disk (SCSI), click on the ADD button, click on the Hard Disk under the Hardware Type on the Add Hardware Wizard Window, select NVMe, in the Add Hardware Wizard window select Create a new virtual disk, on the Specify Disk Capacity add 100GB and make sure that Split virtual disk into multiple files is selected, on the Specify Disk File select a place and then click on the Finish button
- Logon back in into your Windows Server 2022 virtual machine and login as sally
- Rerun the RDP session from sally and re-execute Mimikatz like you did before
- Double click on Exterro FTK Imager launcher, click on the File tab, click on Create Disk Image, select Physical Drive and then click the Next Button, on the Source Drive Selection select an available drive and click on the Finish button, on the create Image Type window click on the Add button, select Raw (dd), on the Evidence Item Information window give it a case number, Evidence Number, Unique Description, Examiner and Notes if you like, on the Image Destination Folder select the 100GB drive that you created before, and then select on the Finish button, then select Start on the Create Image window
- and let the process finish, once it finished you will see a window with hashes so you can confirm if anything is tampered with, the hash will come out different and void the integrity of the image that was created, this is a checksum or list of hashse

## Analyzing the Forensic Image created by FTK
- Click on the File tab, click on Add Evidence Item, on the Select Source window click on Physical Drive and click on the next button, on the Source Drive Selection selec the available drive, select the first file that you see, once its down you will see on the left side under the Evidence Tree section of FTK the Image that was created
- Follow this path: The name of the Image that was created -> Basic dta partition (largest file size) -> NONAME [NTFS] -> root
- You will see a folder named aaAntiRansomElastic-DO-NOT-TOUCH-etc...., this is where Elastic puts early warning detectors, including detections for ransomware 
- Click on the Users folder, click on sally, click on Downloads, you will see that mimikatz was downloaded there and also quickbuck.exe
- In the Desktop Folder you can see the ransomware letter that was created as well

## Analyzing memory dumps with FTK
- Click on File and then click on Capture Memory..., on the Memory Capture window click on Browse and pick a destination, and then click on the Capture Memory button and let the process finish
- Click on the Folder where you dumped the memory dump, click on file name that ends in .mem, you will see the bottom section populate, click on it and then press CTL+F, in the Find window type in Mimikatz and you will see multiple instances of Mimikatz

## Forensics on sally browser history
- Follow this path: sally -> AppData -> Local -> Microsoft -> Edge -> User Data -> Default -> then on the right panel scroll down and find the history file, right click on it and select export, pick a location to export that file
- Go to this link: https://sqlitebrowser.org/dl/
- Go to this link: https://sqlitebrowser.org/dl/
- Download it and unzip it
- Double click on DB Browser (SQLCipher)
- Click on File tab and click on new database and find the history file and open it
- Right click on urls and select Browse Table, you can see the url's that sally visited, including the downloading Mimikatz

## Incident Recovery
- To recover back to a safe state for the Windows Server 2022 Virtual Machine, click on VM tab, click on Snapshot, click on Snapshot Manager, you will see a graph of where you are and the previous state of the virtual machine, click on the previous state and then click on the Go To button




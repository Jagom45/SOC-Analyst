# Using Nessus against the LAN
- On your kali linux virtual machine download Nessus, Download link: https://www.tenable.com/downloads/nessus?loginAttempted=true
- Select the latest version of Nessus and select Linux-Debian-amd64
- On the License Agreement select I Agree
- CD to /Downloads in Kali linux
- Type in the following command: apt install ./Nessus-10.12.5-debian10_amd64.deb
- On your kali linux virtual machine go to this link: https://www.tenable.com/products/nessus/nessus-essentials
- Fill out the Register for your 30-day free license
- The code will be in the email
- Go back to your kali linux virtual machine and type the following command: systemctl start nessusd
- Open Firefox and type in the following URL:127.0.0.1:8834
- On the tenable Nessus webpage select Register Offline and click on Continue
- Copy the challenge code and click on Offline Registration link
- Copy and past the challenge code in the first field and the code from your email in the second field
- Copy the Begin TENABLE LICENSE
- On the tenable Nessus Professional webpage past the Begin TENABLE LICENSE under the Nessus License Key and press on Continue
- Fill in the Username and Password and click on Submit

# Nessus updating plugins
- Click on settings, click on software update, select update plugins and click on continue and click on save
- Click on Manual Software Update, click on update plugins and then click on Continue

# Nessus setup
- Click on scans
- Under the Welcome to Nessus Essentials, type in the network that you want to scan
- Example: 192.168.1.1-192.168.1.5 or 192.168.2.0/24

# Downloading Windows 10
- Go to this link: https://www.microsoft.com/en-us/software-download/windows10#d2784474-fdb0-4e9d-9e47-5e88c0e053ec
- Under Create Windows 10 installation media click on Download Now
- Go to your download folder and double click on the Media Creation Tool
- On the Application notices and license term click on Accept
- On the What do you want to do window click on Create installation media
- On the Select language, architecture, and edition click on Next
- On the Choose which media to use click on ISO file
- Pick a place to download it to
- On the Burn the ISO file to a DVD click on Finish
- Let the installation finish

# Installing Windows 10 on a VM
- Click on File and then on New
- Select Typical and then click on Next
- Select I will install the operating system later and click on Next
- Select Microsoft Windows and select Windows 10 x64 for Version
- Give it a Name and click on Next and pick a location
- Select Split virtual disk into multiple files and pick 60GB for the Maximum disk size
- Click Customize Hardware and then click on Network Adapter and then click on Remove
- Click on CD/DVD (SATA) and click on the Windows 10 ISO image
- Click on memory put 4096MB and then click on Close
- Click Finish on the Ready to Create Virtual Machine window

# Installing Windows 10
- Click on Power on this virtual machine
- Press on any key
- On the first window click on Next
- Click Install now
- Click on I don't have a product key
- Click on Windows 10 Pro
- Click on I accept the license terms and then click on Next
- Click on Custom Install Windows only (advanced)
- Click on Next for Where do you want to install Windows
- Let the installation finish

# Windows 10 Setup
- Pick a region and select Next
- Pick a keyboard layout and click on Yes
- On second keyboard layout click on Skip
- On Let's connect you to a network click on I don't have internet
- On There's more to discover when you connect to the internet click on Continue with limited setup
- Pick a name and click on Next
- For the password pick something really easy, just lowercase letters and confirm the password again
- Fill out the security questions
- For Choose privacy settings for your device, turn off all of them and click on Accept
- On Let Cortana help you get things done, click on Now now
- Let the installation finish
- Click on VM and click on Install VM tools
- Install the VM tools and click Next on all of the windows
- Once it finished click on Finish
- Click Yes on the restart window

# Running wmic qfe get hotfixid & systeminfo.exe
- Click on the search bar and type in CMD
- Type in: wmic qfe get hotfixid
- What does wmic qfe get hotfixid do?
- wmic - Launches the Windows Management Instruction Command-line utility
- qfe - Queries Quick Fix Engineering, which provides informaiton about certain installed updates
- get hotfixid - Displays only the hotfix ID fields
- Copy and paste the output into notepad on the computer host that is hosting the VM's
- Next type: systeminfo.exe
- What does systeminfo.exe do?
- Collects and displays system info and it does not update anything
- Note: An adversary may run these commands after obtaining access to a victim host to determine its patch state an the likelihood they can escalate privileges.

# Disabling updates 
- Go to Advanced Options under Windows updates
- Under Pause updates select the maximum date

# Enable Remote Desktop
- Go to settings and click on System
- Click on Remote Desktop and Enable Remote Desktop
- Reconnect the windows 10 VM back to the internet by adding a network adapter under the VM settings
- Select Bridge and replicate the physical network connection state
- Select Yes for being discoverable in windows

# Scanning with Nessus
- Go back to Nessus and click on New Scan
- Click on Basic Network Scan and name it Scan1
- In the targets section input the IP addresses of the Domain Controller and of the Windows 10 VM with a comma between each IP address
- Click on Save and then click on the Arrow button to launch it
- Let the scan finish
- Once its down you can see the results by clicking on the name of the scan





 


























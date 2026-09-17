# System Changes and Elastic Agent Installation
- This section documents the installation and setup for an elastic agent on Windows Server 2022 along with some system changes to Windows Server 2022.

## System Changes to Windows Server 2022
- Open up Vmware Workstation, use the tab to click on Windows Server 2022, click on Power on this virtual machine
- Click on the VM tab on VMware Workstation and then click on Send Ctl+Alt+Del to get passed the lock screen
- On the login page type in your password and press enter
- If the network discovery notification pops up press yes
- Click on the search box on the bottom left and type in run and then type in gpedit.msc
- On the Local Group Policy Editor, click on Computer Configuration, click on Administrative Templates, click on Windows Components, click on Windows Update
- On the right hand side of the Local Group Policy Editor window double click on Configure Automatic Updates
- On the Configure Automatic Updates click on Disabled and then click on the Apply button and then click on the OK button


  
## Installing an Elastic Agent on Windows Server 2022
- Open up VMware Workstation, use the tab to click on Security Onion 2, click on Power on this virtual machine
- On the login page enter the username and password and press Enter
- You will see Access the Security Onion web interface at http: IP Address, use Chrome or Edge to access that address, it will take you to the Security Onion 2 portal, access it on the Windows Server 2022 virtual machines using Chrome or Edge
- In the Security Onion 2 virtual machines type in sudo so-status and type in the password, it will display the Security Onion Status in green text and it should say running
- In the Windows Server 2022 on the bottom right click on the search bar and type in cmd, in the cmd prompt type in ipconfig to see the virtual machine IP address
- In the Windows Server 2022 Click on Edge, on the Welcome page, click on Start without your date, click on Confirm and continue, click on Continue without google data, click on Confirm and continue, click on Confirm and start browsing
- Type the IP address that was shown in Security Onion 2 in Chrome or Edge
- On the login Webpage, enter the email and password
- On the top left side click on the 3 bars, click on Administration, click on Configuraiton, click on Firewall, click on host groups, for elastic_agent_endpoint, fleet and manager type in the IP address of Windows Server 2022 and then click on the green check
- Click on the 3 bars, click on Downloads and click on Windows x_64 Installer (EXE)
- On the Downloads popup notification, click on it click on the 3 dots, click on keep, on the Make sure you trust notification, click on the arrow pointing down and then click on keep anyway, click on the folders icon on the Downloads popup
- Click on the Elastic Agent file and right click on it, click on Administrator
- Let the installation process finish
- In the Downloads folder you will see a file named SO-Elastic-Agent_Installer, click on it and it should say Elastic Agent installation completed

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
- In the Windows Server 2022 virtual machine type the IP address that was shown in Security Onion 2 in Chrome or Edge
- On the login Webpage, enter the email and password
- On the top left side click on the 3 bars
- Look for Downloads and click on Windows x_64 Installer (EXE)
- 

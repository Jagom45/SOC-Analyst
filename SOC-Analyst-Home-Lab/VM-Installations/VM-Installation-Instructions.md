# VM Installation

This section documents the installation and setup for the virtual machines in a SOC lab.

## Virtual Machine Software Links

- Here is the link for Virtual Machine Software:
- https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion

## Operating System Links

- Here is the link for Windows Server 2022:
- https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022

- Here is the link for Security Onion 2:
- https://docs.securityonion.net/en/2.4/download.html
- https://download.securityonion.net/file/securityonion/securityonion-2.4.211-20260407.iso

- Here is the link for Kali Linux:
- https://www.kali.org/get-kali/#kali-virtual-machines

## Downloading VMware
- Look for VMware and click the download button under VMware
- It will now download to your Downloads folder

## Downloading Security Onion 2
- Look for for this link and click on it: https://github.com/Security-Onion-Solutions/securityonion/blob/2.4/main/DOWNLOAD_AND_VERIFY_ISO.md
- Look for this link and click on: https://download.securityonion.net/file/securityonion/securityonion-2.4.211-20260407.iso
- It will now download to your Downloads folder

## Downloading Windows Server 2022
- Look for ISO downloads 64-bit edition link
- Click on the link and it will now download to your Downloads folder

## Installing Windows Server 2022 in VMware Workstation
- Right click on VMware Workstation Pro and Run as administrator
- Click Yes on the pop up window
- Click on File in the tabs section
- Click on new Virtual Machine
- In the New Virtual Machine Wizard window, click on Typical (recommended) option, click next
- In the Guest Operation System Installation window, click on I will install the operation system later, click on next
- In the Select a Guest Operating System window, click on Microsoft Windows, for the version drop down select Windows Server 2022, click on next
- In the Name the Virtual Machine window, name your virtual machine, for Location pick a place to install the VM, click next
- In the Specify Disk Capacity window, type in 60.0, select Split virtual disk into multiple files, click next
- In the Ready to Create Virtual Machine window, click on Customize Hardware, click on memory and type in 2048MB in the Memory for this virtual machine,
- Click on processors and for Number of processors select 1 in the drop down, for Number of core per processor select 2 in the drop down
- Click on NEW CD/DVD (SATA) and then select Use ISO image file, click on Browse and find and select the Windows Server 2022 ISO file in your Downloads folder
- Click on Network Adapter and select Bridged and click on Replicate Physical Connection State, click on close
- On the Ready to Create Virtual Machine click on Finish

## Installing Windows Server 2022 ISO image on VMWare Workstation
- Click on Power on this virtual machine
- On the Press any key to boot from CD or DVD.., press Enter on your keyboard
- On the Microsoft Server Operating System Setup click next
- On the next window click on Install now button
- On the Select the operation system you want to install window, select Windows Server 2022 Standard Evaluation (Deskop Experience) and click next
- On the Applicable notice and license terms select the check box I accept the Microsoft Software License Terms and then click next
- On Which type of installation do you want click on Custom Install Microsoft Server Operation System only (advanced)
- On the next window, click on Drive Unallocated Space, it should say 60.0GB and click on next
- On the next window, let the setup process finish on its own
- On the Customize Settings window, type in a strong password that is bare minimum of 12 characters or longer
- You have now successfully installed Windows Server 2022, now in the VMware Workstation tab, click on the VM tab, then click on Send Ctl+Atl+Del to get passed the lockscreen
- On the Administrator window type in the password that you decided on
- On the Networks notification click on yes

## Installing Security Onion 2 ISO image on VMWare Workstation
- Right click on VMware Workstation Pro and Run as administrator
- Click Yes on the pop up window
- Click on File in the tabs section
- Click on new Virtual Machine
- In the New Virtual Machine Wizard window, click on Typical (recommended) option, click next
- On Guest Operation System Installation select Installer disk Image file (iso):, click on Browse button and find and select the Security Onion Server ISO file in your Download folder, click next
- On Name the Virtual Machine window, for Virtual machine name pick a name for your VM, for Location pick a place to install the VM, click next
- In the Specify Disk Capacity window, type in 200.0, select Split virtual disk into multiple files, click next
- In the Ready to Create Virtual Machine window, click on Customize Hardware, click on memory and type in 16384MB in the Memory for this virtual machine,
- Click on processors and for Number of processors select 1 in the drop down, for Number of core per processor select 8 in the drop down
- Click on Network Adapter and select NAT and click on Replicate Physical Conenction State
- Click on Add button at the bottom to add a new Network Adapter, on the Hardware Type window click on Network Adapter and click on finished, you will see Network Adapter 2, click on Network Adapter 2 and select Bridge and click on Replicate Physical Connection State, click on close
- On the Ready to Create Virtual Machine click on Finish

## Installing Security Onion 2 ISO image on VMWare Workstation
- Click on Power on this virtual machine
- On the WARNING window, type in yes and press enter
- For Enter an administrative username pick a name and press enter
- For Let's set a password for the me user, pick a password and press enter
- For Enter a password, pick a password and press enter
- For Re-enter the password, retype the previous password that you picked
- Let the installation process finish
- On the Initial Install Complete page, press Enter to reboot
- On the localhost login, type in the username and press enter and then type in the password and press enter
- On the Security Onion Setup use the TAB button to select yes and press enter
- On the select an option use the up and down arrow buttons to select Install Run the Standard Security Onion installation and then use tab to select ok
- On the What kind of installation would you like to do, use the up and down arrows to select STANDALONE Standalone production install and then use the tab button to select ok
- On the Elastic Stack binaries page, type in AGREE and then use the tab button to select ok
- On How should this node be installed, use the up and down button to select Standard This node has access to the internet and then use the tab button to select ok
- On Enter the hostname (not FQDN) you would like to set, type in a host name and then use the tab button to select ok
- On the Enter a short description for the nose or press Enter to leave blank, go ahead and just select ok with the tab button
- On the Please select the NIC you would like to use for management, use the arrow key to select the first NIC
- On the Choose how to set up your management interface, use the arrow key to select STATIC Set a static IPv4 address (recommended) and then use the tab button to select ok
- On the What IPv4 address would you like to assign to this Security Onion installation, click on the Edit tab on VMware Workstation, click on Virtual Network Editor and then click on the one that says NAT, click on NAT Setting button next to NAT (shared host's IP address with VMs), look at the Gateway IP address and at the Subnet mask, pick an IP address within that range in CIDR notation, then use tab to select ok
- On the Enter your gateway's IPv4 address, enter the gateway's IPv4 address from the previous step, use the tab button to select ok
- On the Enter your DNS servers separated by commas page, use the tab button to select ok
- On the How would you like to connect to the Internet, use the arrow button to select Direct and then use the tab button to select ok
- On the Do you want to keep the default Docker IP range page, use the tab button to select Yes and press enter
- On the Please add NIC's to the Monitor Interface, use the space bar to select the NIC, then use the tab button to select ok and press enter
- On the Please enter an email address to create an administrator page, enter an email address and then use the tab button to select ok
- On the Enter a password for the email that you picked, enter a password, then use the tab button to select ok
- On the Re-enter a password for the email that you picked, re-enter the password that you picked and then use the tab button to select ok
- On How would you like to access the web interface page, use the arrow keys to select IP Use IP address to access the web interface and then use the tab button to select ok
- On the Do you want to allow access to this Security Onion installation via the web interface, use the tab button to select yes
- On the single IP address or an IP range, in CIDR notation, to allow, enter an IP range with CIDR notation and then use the tab button to select ok
- On The Security Onion development team could use your help page, use the tab button to select yes
- On the following options have been set, would you like to proceed page, use the tab button to select yes
- Let the installtion process finish
- On the STANDALONE setup is now complete page, use the tab button to select ok, also make sure to use the IP address to enter the portal for Security Onion 2
- If you like you can use the command sudo su so-status to see if the containers in Security Onion 2 are running, it should say running in green text



**Installing for Kali linux
- Go to your download folder and install VMware Workstation
- Next click on file, new virtual machine, next, click on browse and find the ISO image, click next for Easy Install Information, name your virtual machine and click next, select 40GB for maximum disk size and click one Split virtual disk into multiple files, click finish, follow the installation process

**Installing for Security Onion 2
- Go to your download folder and install VMware Workstation
- Next click on file, new virtual machine, next, click on browse and find the ISO image, click next for Easy Install Information, name your virtual machine and click next, select 200GB for maximum disk size and click one Split virtual disk into multiple files, under Ready to Create Virtual Machine click on Customize Hardware, under memory put 16GB, under processor select 8 cores, click finish, follow the installation process

- 

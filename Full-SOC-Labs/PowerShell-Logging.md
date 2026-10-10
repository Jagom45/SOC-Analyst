# PowerShell
- This lab will show how to log PowerShell

# Server 2022
- Press Win + R
- Type gpedit.msc
- Computer Configuration → Administrative Templates → Windows Components → Windows PowerShell
- Click on Turn on PowerShell Script Block Logging
- Select Enabled

# Launching PowerShell
- Running PowerShell
- Launch PowerShell and launch it
- Type in notepad

# EventViewer Event ID 4140
- Navigate to here: Applications and Services Logs → Microsoft → Windows → PowerShell → Operational
- You will see a 4104 Event with the description of Execute a Remote Command with a description that says notepad
- Creating Scriptblock text (1 of 1):
- notepad
- ScriptBlock ID: cd8dfe11-43bc-40de-88d3-dbc3b930563c
- Path: 

# EventViewwer Event ID 4688
- Navigate to = Windows Logs → Security
- Click on Filter Current log and type in 4688 and press on okay
- Click on Find and then type in powershell.exe

- A new process has been created.

- Creator Subject:
	- Security ID:		cs\Administrator
	- Account Name:		Administrator
	- Account Domain:		cs
	- Logon ID:		0x99C2C

- Target Subject:
	- Security ID:		NULL SID
	- Account Name:		-
	- Account Domain:		-
	- Logon ID:		0x0

- Process Information:
	- New Process ID:		0x1290
	- New Process Name:	C:\Windows\System32\notepad.exe
	- Token Elevation Type:	TokenElevationTypeDefault (1)
	- Mandatory Label:		Mandatory Label\High Mandatory Level
	- Creator Process ID:	0x2780
	- Creator Process Name:	C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
	- Process Command Line:	"C:\Windows\system32\notepad.exe"

# PowerShell with Event ID 4104
- Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Powershell/Operational'; Id=4104} -MaxEvents 500 | Where-Object {$_.Message -match 'notepad'} | Select-Object TimeCreated, Message | Format-List
- TimeCreated : 10/10/2026 1:44:46 PM
- Message     : Creating Scriptblock text (1 of 1):
                notepad
# PowerShell with Event ID 4688
- Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4688} -MaxEvents 100 | Where-Object { $_.ToXml() -like '*notepad.exe*' } | Select-Object TimeCreated, Message | Format-List
- TimeCreated : 10/10/2026 3:16:52 PM
- Message     : A new process has been created.

              Creator Subject:
                Security ID:            S-1-5-21-353739892-4176304805-2976134450-500
                Account Name:           Administrator
                Account Domain:         cs
                Logon ID:               0x99C2C

              Target Subject:
                Security ID:            S-1-0-0
                Account Name:           -
                Account Domain:         -
                Logon ID:               0x0

              Process Information:
                New Process ID:         0x1ce0
                New Process Name:       C:\Windows\System32\notepad.exe
                Token Elevation Type:   TokenElevationTypeDefault (1)
                Mandatory Label:                S-1-16-12288
                Creator Process ID:     0x28cc
                Creator Process Name:   C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
                Process Command Line:   "C:\Windows\system32\notepad.exe"

              Token Elevation Type indicates the type of token that was assigned to the new process in accordance with User Account Control policy.

              Type 1 is a full token with no privileges removed or groups disabled.  A full token is only used if User Account Control is disabled or if the user is the built-in Administrator account or a service account.

              Type 2 is an elevated token with no privileges removed or groups disabled.  An elevated token is used when User Account Control is enabled and the user chooses to start the program using Run as administrator.  An elevated token is also used when an application is configured to always require administrative privilege or to always require maximum privilege, and the user is a member of the Administrators group.

              Type 3 is a limited token with administrative privileges removed and administrative groups disabled.  The limited token is used when User Account Control is enabled, the application does not require administrative privilege, and the user does not choose to start the program using Run as a


# Ticket

Who: Which user logs in, runs the command, or downloads the file
Who: Administrator who belongs to the cs domain
what: launched PowerShell and launched notepad.exe from PowerShell
When: This happened at 1:44:46 PM
Where: This happened on WIN-F73S2MC5TMC.cs.org
Why: My conclusion is that this is a True Negative because this was done in a controlled lab environment

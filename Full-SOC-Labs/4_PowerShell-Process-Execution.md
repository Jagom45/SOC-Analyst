# Creating a malicious powershell script to trigger an alert event in event viewer
- Type in run in the search bar
- secpol.msc
- Computer Configuration -> Windows Setting -> Security Settings -> Advanced Audit Policy Configuration -> Sysetm Audit Policies -> Detailing Tracking -> Audit Process Creation
- Go back to the run application
- Type in gpedit.msc
- Computer Configuration -> Administrative Template -> System -> Audit -> Process Creation
- Select Enabled

# CMD
- Launch CMD and type in: gpupdate /force

# Powershell
- Type in: powershell.exe -NoProfile -Command "Write-Output 'SOC-LAB-PowerShell-Test'"
- What the command does:
- powershell.exe — starts Windows PowerShell
- NoProfile — prevents PowerShell from loading the user's profile scripts/configuration. This makes the execution more predictable
- Command — tells PowerShell to execute the following command
- Write-Output 'SOC-LAB-PowerShell-Test' — writes the text SOC-LAB-PowerShell-Test to standard output

# Event Viewer 
- Go to event viewer
- Find an event tht says 4688 Process Creation:
A new process has been created.

Creator Subject:
	Security ID:		cs\Administrator
	Account Name:		Administrator
	Account Domain:		cs
	Logon ID:		0x9152A

Target Subject:
	Security ID:		NULL SID
	Account Name:		-
	Account Domain:		-
	Logon ID:		0x0

Process Information:
	New Process ID:		0x2180
	New Process Name:	C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
	Token Elevation Type:	TokenElevationTypeDefault (1)
	Mandatory Label:		Mandatory Label\High Mandatory Level
	Creator Process ID:	0x1428
	Creator Process Name:	C:\Windows\System32\cmd.exe
	Process Command Line:	powershell.exe  -NoProfile -Command "Write-Output 'SOC-LAB-PowerShell-Test'"
- The command that was run above is shown here


# Ticket

Title: Event ID 4688 Process Creation

Descriptino: A new process has been created 

Timestamp: 10/7/2026 1:36:16 PM


Triage: An Event ID of 4688 Process was created. I investigated the alert and saw that it was made under the accoutn name of Administrator that belongs to the account domain of cs. A new process was created with CMD.exe and launched Powershell.exe. On Powershell.exe the command: powershell.exe  -NoProfile -Command "Write-Output 'SOC-LAB-PowerShell-Test'" was run

Analysis: After analyzing the event and the command that was executed under powershell, the command make Powershell write the text SOC-LAB-PowerShell-Test to the screen. There is strong evidence that a CMD process was used to create Powershell.exe and excute those lines of text.

Conclusion: No harm was intented with that command in Powershell.exe, the event is flagged as True Negative as the event did show up in event viewer but it was not a malicious intent and there is no evidence of malicious activity

Escalation: No escalation was required

Action: The alert was investigated using the event viewer, looked over the parent and the process that was created by the parent process. Looked over at the command that was used and saw that the command printed text to the screen using powershell.

Recommendation: Continue monitoring the computer to see if powershell.exe is used to cause harm

Status: Closed/True Negative

Evidence:
Event Viewer Screenshot 

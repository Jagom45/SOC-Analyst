# PowerShell-Project.md

- This is a PowerShell Project experimenting with different commands

# Command 1
- Get-Date
- Displays Date, Month, Number, Year, Hour, Minute, Second, AM OR PM
- Thursday, October 8, 2026 11:44:43 PM

# Command 2
- $env:computername
- Displays the name of the computer

# Command 3
- Get-Process
- Lists all of the process running on the computer organized by Handles, NPM(K), PM(k), WS(K), CPU(s), Id, SI, ProcessName
- 744      35   105968     133316      18.66  21164   1 powershell
- ID - Means Process ID(PID), its a unique number windows assigns to a running process

# Command 4
- Get-Process -Name PowerShell
- Lists all of the process with the name PowerShell in it

- Command 5
- Get-Process -Id 10012
- List the information about the process with ID 21164
- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   5808     162   273196     301412     200.45  21164   1 powershell

# Command 6
- Get-Process -Name | Select-Object Id, ProcessName
- 
PS C:\Users\Me\Desktop> Get-Process -Name powershell | Select-Object Id, ProcessName

-   Id ProcessName
   -- -----------
21164 powershell
24144 powershell

# Command 7
- (Get-Process Id 21164).ProcessName
- Displays a specific process by its ID, the .ProcessName part tells PowerShell to display only the process name instead of all of its details
- powershell

# Command 8
- Get-Process -Id 21164 | Select-Object Id, ProcessName, StartTime
- StartTime asks PowerShell to show when the process began running
- Select-Object lets us choose which details to display
-   Id ProcessName StartTime
   -- ----------- ---------
21164 powershell  10/8/2026 11:10:39 PM

# Command 9
- Get-Process -Id 21164 | Select-Object ProcessName, CPU
- Displays how much processor time that process has used since it started
- Id ProcessName    CPU
   -- -----------    ---
21164 powershell  19.125


# Command 10
- Get-Process | Sort-Object CPU -Descending
- Get-Process - gets the running process
- | - passes the results to the next command
- Sort-Object CPU - arranges process by CPU time
- Descending - puts the highest CPU times first

- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2560     127   656804     387472   2,015.89   4488   1 chrome

# Command 11
- Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
- Get-Process — gets the running processes.
- Sort-Object CPU -Descending — sorts by CPU time, highest first
- Select-Object -First 5 — keeps only the first five results
- First 5 - Display the first 5 of that list

- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2558     127   701360     391612   2,018.84   4488   1 chrome
   2380     101   361160     479344   1,894.30   4612   1 chrome
    597      65   584488     631880     642.92  21812   1 chrome
    550      53   108020      92772     562.56   7388   1 chrome
    396      34   136584     154928     507.25  13180   1 chrome

# Command 12
- Get-Process | Where-Object { $_.CPU -gt 10 }
- Where-Object - filter the results
- $_ - represents the current process being checked
- .cpu - refers to that process's CPU time
- -gt 10 - means "greater than 10"
- The spaces before the $_ and after the 10 are optional

- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2376     100   361508     459948   1,904.09   4612   1 chrome
    418      27    65556      75704      16.16   5856   1 chrome
    554      54   108084      91660     563.48   7388   1 chrome

# Command 13
- Get-Process | Where-Object { $_.CPU -lt 10 }
- -lt 10 - Display anything with less than 10 seconds of cpu time
- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    210      11     1772      10880              8004   0 A
    227      14     4180      12224             14832   0 A

# Command 14
- Get-Process | Where Object { $_.ProcessName -eq "chrome" }
- Get-Process - get the running process
- Where-Object - filters them
- ProcessName - Checks each process's Name
- -eq "chrome" - keeps only process name chrome
- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2560     127   634024     372212   2,029.17   4488   1 chrome
   2388     103   361940     459960   1,911.50   4612   1 chrome


# Command 15
- Get-Process | Where_Object { $_.ProcessName -ne "chrome" }
- -ne "crhome" - All process except Chrome
- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    210      11     1772      10480              8004   0 A
    227      14     4184      12232             14832   0 A
# Command 16
- Get-Process | Where-Object { $_.CPU -gt 10 } | Sort-Object CPU -Descending
- Get-Process - gets running process
- Where-Object { $_.CPU -gt 10 }
- Sort-Object CPU -Descending
- Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2557     127   655224     368864   2,033.00   4488   1 chrome
   2384     102   362200     457784   1,917.91   4612   1 chrome

# Command 17
- $highCPU = Get-Process | Where-Object { $_.CPU -gt 10 }
- $higherCPU - Is the variable we're creating
- = - Stores the results on the right in that variable
- You won't see any output
- If you run the command again, the variable will get updated with new results

# Command 18
- $highCPU.Count
- .Count - Counts how many results are in a variable
- That means that the variable contains 12 process results
- 31

- Command 19
- $highCPU | Select-Object -Property ProcessName
- $highCPU - Supplies your saved process results
- Select-Objects - chooses which information to display
- Property ProcessName - Displays just the process name instead of all the columns
- ProcessName
-----------
A
A

# Command 20
- $highCPU | Select-Object -Property ProcessName
- Does the same as the command above

# Command 21 
- $highCPU | Select-Object -ExpandProperty ProcessName | Sort-Object -Unique
- Sort-Object -Unique - sorts the names and removes duplicates

# Command 22
- ($highCPU | Select-Object -ExpandProperty ProcessName | Sort-Object -Unique).count
- If you use the other brackets, you get 1
- 17

# Command 23
- (Get-Process | Where-Object { $_.ProcessName -eq "chrome" }).Count
- Get-Process gets running process
- Where-Object - keeps only process named chrome
- .Count counts how many Chrome process were found

# Command 24
- Get-Process -Name chrome | Select-Object ProcessName, Id, StartTime
- Get-Process - Retrieves information about running processes
- -Name chrome - Tells PowerShell to retrieve process named chrome
- | passes those process object to the next command
- Select-Object - choose which properties to display
- ProcessName - Displays the process name
- Id - displays the process ID (PID), which helps distinguish individual process instances
- StartTime - Displays when each process started, if that information is available
- ProcessName    Id StartTime
-----------    -- ---------
chrome       2856 10/9/2026 11:14:01 PM
chrome       4444 10/9/2026 11:43:00 PM
chrome       4488 10/7/2026 10:49:45 AM

# Command 25
- Get-Process -Name chrome | Select-Object ProcessName, Id, Startime | Sort-Object StartTime -Descending
- Get-Process -Name chrome - Retrieves Chrome processes
- | passes the results to the next command
- Sort-Object StartTime - sorts the process by their start time
- -Descending - puts the latest start times first
- PowerShell attempts to sort the incoming objects using their default sorting behavior if it was just Sort-Object -Descending

# Command 26
- Get-Process -Name chrome | Sort-Object StartTime -Descending | Select-Object -First 5 ProcessName, Id, StartTime
- 
- ProcessName    Id StartTime
-----------    -- ---------
chrome       7704 10/10/2026 12:23:52 AM
chrome       4444 10/9/2026 11:43:00 PM
chrome      24772 10/9/2026 11:42:58 PM
chrome      19644 10/9/2026 11:41:04 PM


# Event Viewer
- Commands with Event Viewer

# Command 1
- Get-WinEvent -Listlog *
- Get-WinEvent - retrieves Windows events
- -LogName Security - Specifies the Security log
- * - List all
- Shows all of the logs

# Command 2
- Get-WinEvent -ListLog Security
- LogMode - Indicates how Windows manages the event log, such as whether it overwrites older events when needed
- MaximumSizeInBytes - The maximum storage size configured for the log, measured in bytes
- RecordCount - The number of event records currently stored in the log
- LogName - The name of the log, which should be Security
- LogMode   MaximumSizeInBytes RecordCount LogName
-------   ------------------ ----------- -------
Circular            20971520       30954 Security

# Command 3
-Get_WinEvent -LogName Security -MaxEvents 5
- TimeCreated - The date and time Windows recorded the event
- Id - The Windows event ID, which helps identify the type of event
- LevelDisplayName - The event's severity or level, such as Information, Warning, or Error, not every security event uses this field
- Message - A description of what happened


-ProviderName: Microsoft-Windows-Security-Auditing
TimeCreated                      Id LevelDisplayName Message
-----------                      -- ---------------- -------
10/10/2026 12:51:03 AM         4672 Information      Special privileges assigned to new logon....
10/10/2026 12:51:03 AM         4624 Information      An account was successfully logged on....

# Command 4
- Get-WinEvent -LogName Security -MaxEvents 5 | Select-Object Id
- Filters by Id field
- Id
  --
4672
4624

# Command 5
- Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 5
- Get-WinEvent - retrieves windows event records
- -FilterHashtable - lets you filter events using several conditions at once
- @{ ... } creates a collection of named settings for the filter
- LogName='Security' - tells PowerShell to search the Security log
- Id=4625 - restricts the results to failed logon events
- -MaxEvent 5 - Limits the output to five matching events

- ProviderName: Microsoft-Windows-Security-Auditing
TimeCreated                      Id LevelDisplayName Message
-----------                      -- ---------------- -------
10/4/2026 5:57:16 PM           4625 Information      An account failed to log on....
10/4/2026 4:05:26 PM           4625 Information      An account failed to log on....

# Command 6
- Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 5 | Select-Object TimeCreated, Message
- Get-WinEvent - Retrieves windows events
- FilterHashtable @{LogName='Security'; Id=4625} filters for failed logon events in the Security log
- MaxEvents 5 limits the results to five events.
- | passes those events to the next command
- Select-Object TimeCreated, Message displays when each event occurred and its detailed message


-TimeCreate Message
---------- -------
An account failed to log on....
An account failed to log on....

# Command 7
- Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625; StartTime=(Get-Date).AddHours(-1000)} -MaxEvents 10
- LogName — which event log to search.
- Id — which event ID to look for.
- StartTime — the earliest event time to include
- Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625; StartTime=(Get-Date).AddHours(-1)} -MaxEvents 10
- StartTime - sets the earliest time for matching events.
- Get-Date - retrieves the current date and time.
- .AddHours(-1) calculates the time one hour before now. The -1 means subtract one hour.
- MaxEvents 10 limits the output to ten matching events.

- ProviderName: Microsoft-Windows-Security-Auditing
TimeCreated                      Id LevelDisplayName Message
-----------                      -- ---------------- -------
10/4/2026 5:57:16 PM           4625 Information      An account failed to log on....
10/4/2026 4:05:26 PM           4625 Information      An account failed to log on....


# Command 8
- Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Powershell/Operational'; Id=4104} -MaxEvents 5 | Select-Object TimeCreated, Id, Message | Format-List

















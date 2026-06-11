# 05 - PowerShell Analysis

## Overview
PowerShell is one of the most abused tools by attackers because it is 
trusted, powerful and built into Windows. Sysmon Event 1 only captures 
that powershell.exe was launched, not the commands executed inside. 
The PowerShell history file fills that gap.

## History File Location
C:\Users<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

## Key Characteristics
- Created automatically for each user
- Survives system reboots unless manually deleted
- Records every command entered in a PowerShell session
- Does not record command output
- Does not record script content when running .ps1 files

## How to Read It
```powershell
cat C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

## Red Flags
- Invoke-WebRequest downloading executables to C:\Temp
- Get-LocalUser or Get-LocalGroup — system reconnaissance
- New-LocalUser or Add-LocalGroupMember — persistence
- Encoded commands (-EncodedCommand)
- Commands referencing suspicious paths or external IPs

## Practical Case
Reviewing the PowerShell history of multiple local users on a compromised 
host revealed malicious commands including system reconnaissance and 
a flag embedded in one user's history file.

**Detection steps:**
1. List all local users with Get-LocalUser
2. Read history file for each active user
3. Look for download commands, reconnaissance or persistence techniques
4. Correlate timestamps with Sysmon Event 1 for powershell.exe

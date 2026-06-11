# 07 - File and Registry Changes (Sysmon)

## Overview
Attackers often modify files and registry keys to maintain persistence, 
hide their presence or stage tools for later use. Sysmon Events 11 and 13 
capture these changes in real time.

## Key Event IDs

| Event ID | Description | Malicious Use |
|----------|-------------|---------------|
| Sysmon 11 | File created | Malware dropping files |
| Sysmon 13 | Registry value set | Persistence mechanisms |

## Key Fields
- **TargetFilename** — full path of created file (Event 11)
- **TargetObject** — registry key modified (Event 13)
- **Image** — process that made the change
- **ProcessId** — correlates with Sysmon Event 1

## Red Flags — File Creation
- Files created in temp directories (C:\Temp, C:\Users\Public)
- Executable files (.exe, .bat, .ps1) dropped by unexpected processes
- Files created by browser or Office processes
- Files with random or misleading names

## Red Flags — Registry Changes
- Modifications to autorun keys:
  - HKCU\Software\Microsoft\Windows\CurrentVersion\Run
  - HKLM\Software\Microsoft\Windows\CurrentVersion\Run
- Changes made by unknown or suspicious processes
- Registry modifications outside business hours

## How to Correlate
1. Filter Sysmon Event 11 or 13
2. Copy ProcessId
3. Search Sysmon Event 1 with same ProcessId
4. Identify the parent process and full execution context
5. Check if file or registry key is used for persistence

## Practical Case
After identifying ckjg.exe as malicious via Sysmon Event 1, filtering 
Event 11 with the same ProcessId revealed a file dropped by the malware 
in a suspicious directory — confirming persistence on the compromised host.

**Detection steps:**
1. Filter Sysmon Event 11 with ProcessId of suspicious process
2. Check TargetFilename for suspicious paths or extensions
3. Filter Sysmon Event 13 for registry persistence keys
4. Correlate all findings with the original Event 1
5. Document and escalate

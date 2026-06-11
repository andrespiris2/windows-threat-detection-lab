# 03 - Process Monitoring (Sysmon)

## Overview
Sysmon Event ID 1 captures every process creation on the system, including 
the full command line, parent process, and user context. It is the most 
valuable event for reconstructing attack chains.

## Key Event IDs

| Event ID | Description | Malicious Use |
|----------|-------------|---------------|
| Sysmon 1 | Process created | Detect malicious execution |
| 4688 | Process created (native) | Alternative without Sysmon |

## Key Fields in Sysmon Event 1
- **Image** — full path of the executed process
- **CommandLine** — exact command used
- **ParentImage** — process that launched it
- **User** — account that ran the process
- **LogonId** — correlates with authentication events
- **Hashes** — MD5/SHA256 for threat intel lookup

## Red Flags
- Unexpected process spawned from browser or Office app
- PowerShell or cmd launched by a non-admin user
- Process running from temp directories (C:\Temp, C:\Users\Public)
- Unknown executable with random name (e.g. ckjg.exe)
- Legitimate tool name masking malware (e.g. svchost.exe in wrong path)

## Practical Case
Sysmon Event ID 1 revealed a process named ckjg.exe running from 
C:\Users\sarah.miller\Downloads\. Despite appearing as CxApp, 
the OriginalFileName field showed Stub.exe — a common malware indicator.

**Detection steps:**
1. Filter Sysmon Event ID 1
2. Look for processes in suspicious paths
3. Check OriginalFileName vs Image name mismatch
4. Copy ProcessId — correlate with network and file events
5. Submit hash to VirusTotal for confirmation

# 04 - Network Connections (Sysmon)

## Overview
Sysmon Events 3 and 22 capture outbound network connections and DNS queries 
made by processes. These are critical for detecting C2 communication, 
data exfiltration and malware callbacks.

## Key Event IDs

| Event ID | Description | Malicious Use |
|----------|-------------|---------------|
| Sysmon 3 | Network connection | Detect C2 communication |
| Sysmon 22 | DNS query | Detect malicious domains |

## Key Fields
- **Image** — process making the connection
- **DestinationIp** — target IP address
- **DestinationPort** — target port
- **QueryName** — domain being resolved (Event 22)
- **ProcessId** — correlates with Sysmon Event 1

## Red Flags
- Connection to external IP on port 4444 or non-standard ports
- DNS query to suspicious TLDs (*.top, *.click, *.xyz)
- Network activity from a process in a temp directory
- Connection from a process that should not have network access

## How to Correlate
1. Identify suspicious process in Sysmon Event 1
2. Copy its ProcessId
3. Filter Sysmon Event 3 with the same ProcessId
4. Check DestinationIp and DestinationPort
5. Filter Sysmon Event 22 with same ProcessId
6. Check QueryName for suspicious domains
7. Submit IP or domain to VirusTotal

## Practical Case
After identifying ckjg.exe as malicious via Sysmon Event 1, correlating 
its ProcessId with Event 3 revealed an outbound connection to an external 
IP on a non-standard port. Event 22 showed a DNS query to a .click domain.

**Outcome:** IP blocked at firewall level, process terminated, 
host isolated for forensic analysis.

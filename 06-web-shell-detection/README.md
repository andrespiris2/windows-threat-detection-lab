# 06 - Web Shell Detection

## Overview
A web shell is a malicious script uploaded to a web server that allows 
an attacker to execute commands remotely through a browser. It serves 
both as an initial access method and a persistence mechanism.

## How Attackers Deploy Web Shells
- Exploiting file upload vulnerabilities
- Misconfigured web server permissions
- Prior access to the system

## Key Indicators in Web Server Logs

### Suspicious Request Patterns
- Repeated GET requests scanning for upload directories
- POST requests to file upload locations
- Repeated requests to the same unusual file

### HTTP Methods to Monitor

| Method | Normal Use | Malicious Use |
|--------|------------|---------------|
| GET | Retrieve resource | Reconnaissance, web shell interaction |
| POST | Send data | Upload or interact with web shell |
| PUT | Upload file | Direct web shell upload |

### Suspicious User Agents
- Outdated agents: `Mozilla/4.0 (compatible; MSIE 6.0)`
- CLI tools: `curl/1.XX.X` or `wget/1.XX.X`

### Suspicious Query Strings
- Parameters like `cmd=` or `exec=`
- Base64 encoded commands: `?cmd=d2hvYW1p`
- Unusually long query strings

## Common Web Shell Extensions
- `.php` — Linux/Apache servers
- `.aspx` — Windows/IIS servers

## Real World Examples
- **Hafnium** uploaded .aspx web shells to Exchange servers
- **Conti** ransomware used the same technique to map the network

## Practical Case
Accessed a deployed web shell at /files/awebshell.php and executed 
system commands via the cmd parameter. Listed directory contents with 
ls and retrieved a flag using cat.

**Detection steps:**
1. Review web server access logs for POST requests to upload directories
2. Look for repeated GET/POST to the same .php or .aspx file
3. Check for suspicious User-Agent strings
4. Look for query strings containing cmd= or exec=
5. Submit suspicious file hashes to VirusTotal

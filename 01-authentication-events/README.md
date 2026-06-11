# 01 - Authentication Events

## Overview
Authentication events are among the most valuable log sources for detecting 
unauthorized access, brute force attacks, and lateral movement.

## Key Event IDs

| Event ID | Description | Malicious Use |
|----------|-------------|---------------|
| 4624 | Successful logon | Confirm attacker access |
| 4625 | Failed logon | Brute force detection |
| 4648 | Logon with explicit credentials | Pass-the-hash, lateral movement |

## Logon Types

| Type | Description | Red Flag |
|------|-------------|----------|
| 2 | Interactive (local) | Unusual user or time |
| 3 | Network | Repeated failures |
| 10 | RemoteInteractive (RDP) | External IP |

## Red Flags
- Multiple failed logons (4625) followed by a success (4624)
- Logon Type 10 from an external IP address
- Logons outside business hours
- Logon from an unknown or unexpected geolocation

## Practical Case
During a SOC exercise, multiple failed authentication attempts were detected 
against an administrative VPN account from an IP geolocated outside the normal 
operating region, during non-business hours.

**Detection steps:**
1. Filter Event ID 4625 — identify source IP and targeted account
2. Check logon type — Type 10 confirms RDP attempt
3. Correlate with 4624 — check if any attempt succeeded
4. Copy Logon ID — correlate with subsequent actions in the session
5. Check geolocation of source IP against normal baseline

**Outcome:** Classified as real attack attempt. Contained by blocking IP ranges 
in the firewall, forcing credential rotation and enforcing MFA on VPN access.

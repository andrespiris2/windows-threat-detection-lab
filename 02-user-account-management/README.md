# 02 - User Account Management

## Overview
Attackers often create backdoor accounts or modify existing ones to maintain 
persistent access. Monitoring user management events is critical to detect 
these techniques early.

## Key Event IDs

| Event ID | Description | Malicious Use |
|----------|-------------|---------------|
| 4720 | User account created | Backdoor account creation |
| 4722 | User account enabled | Re-enabling old accounts |
| 4725 | User account disabled | Disabling privileged accounts |
| 4726 | User account deleted | Covering tracks |
| 4732 | User added to security group | Privilege escalation |
| 4733 | User removed from security group | Disrupting response |

## Red Flags
- New account created outside business hours
- Unknown account name or not following naming convention
- Account added to privileged groups like Administrators
- Nobody in IT can confirm the action
- Same Logon ID as a suspicious RDP session (Event 4624)

## How to Correlate
1. Filter Event ID 4720 or 4732
2. Copy the Subject Logon ID
3. Search for Event 4624 with the same Logon ID
4. Identify the session origin — IP, logon type, timestamp

## Practical Case
An attacker logged in via RDP (Event 4624, Logon Type 10) and shortly after 
created a backdoor user account (Event 4720) and added it to two privileged 
security groups (Event 4732).

**Detection steps:**
1. Filter Event 4624 with Logon Type 10 — identify RDP session
2. Copy Logon ID from the session
3. Search Event 4720 with the same Logon ID — find created account
4. Search Event 4732 — confirm which privileged groups were assigned
5. Escalate and contain

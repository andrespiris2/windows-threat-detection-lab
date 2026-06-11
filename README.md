# Windows Threat Detection Lab

A collection of detection use cases based on Windows Event Logs and Sysmon, 
built through hands-on practice in real SOC analysis scenarios.

## About This Project

This repository documents my approach to detecting common attack techniques 
using native Windows logging and Sysmon. Each section covers a specific 
detection category, including the relevant Event IDs, key indicators, 
and a practical case walkthrough.

## Detection Categories

| # | Category | Key Event IDs |
|---|----------|--------------|
| 01 | Authentication Events | 4624, 4625, 4648 |
| 02 | User Account Management | 4720, 4722, 4725, 4726, 4732 |
| 03 | Process Monitoring (Sysmon) | Sysmon 1 |
| 04 | Network Connections | Sysmon 3, 22 |
| 05 | File and Registry Changes | Sysmon 11, 13 |
| 06 | PowerShell Analysis | PowerShell History |
| 07 | Web Shell Detection | Apache/Nginx Access Logs |

## Tools & Environment

- Windows Event Viewer
- Sysmon (Florian Roth config)
- Splunk (SIEM)
- TryHackMe SOC labs

## Certifications

- SAL1 Security Analyst — TryHackMe (2026)
- Master in Cybersecurity — Conquer Blocks (2026)
- CC — ISC2 (in progress)

## Connect

- LinkedIn: linkedin.com/in/andres-piris-ab843796

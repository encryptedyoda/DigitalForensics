# Scheduled Tasks Cheat Sheet

## Purpose
Scheduled Tasks automate program execution and are a common persistence mechanism (MITRE T1053.005).

## Locations
### XML
`C:\Windows\System32\Tasks`

### Registry
`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache`

## Evidence Provided
- Command executed
- Triggers
- Author
- User account
- Last Run
- Next Run

## Investigation Questions
- Is it legitimate?
- Who created it?
- What executable runs?
- Does it launch PowerShell or LOLBins?
- Does it run from AppData or Temp?

## Useful Tools
- Task Scheduler
- Autoruns
- Registry Explorer
- KAPE

## Red Flags
- Encoded PowerShell
- AppData execution
- Temp execution
- Random names
- Hidden tasks

## Example
`npcapwatchdog` running `sc config npcap start= auto` is generally legitimate.

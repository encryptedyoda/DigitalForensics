# Windows Prefetch Cheat Sheet

## Purpose
Prefetch files record application execution to improve startup performance. In DFIR they provide evidence that an executable has been run.

## Location
`C:\Windows\Prefetch`

## Evidence Provided
- Executable name
- Run count
- Last execution time
- Previous execution timestamps (Windows 8+)
- DLLs loaded
- Files accessed
- Volume information

## Investigation Questions
- Did this program execute?
- When was it last executed?
- How many times?
- What files did it access?

## Useful Tools
- PECmd
- WinPrefetchView
- KAPE

## Important Caveats
- Can be disabled.
- Old entries overwritten.
- Presence proves execution.
- Absence does **not** prove non-execution.

## Example
Search for `POWERSHELL.EXE-*.pf`. If found with Run Count 13, PowerShell executed multiple times.

## Quick Commands
`PECmd.exe -d C:\Windows\Prefetch`

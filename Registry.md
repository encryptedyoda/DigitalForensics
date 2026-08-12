# Windows Registry Cheat Sheet

## Purpose
The Windows Registry stores operating system and user configuration.

## Core Hives
- SYSTEM
- SOFTWARE
- SAM
- SECURITY
- DEFAULT
- NTUSER.DAT
- USRCLASS.DAT

## Locations
`C:\Windows\System32\Config`

`C:\Users\<User>\NTUSER.DAT`

`C:\Users\<User>\AppData\Local\Microsoft\Windows\UsrClass.dat`

## Investigation Questions
- Installed software?
- USB devices?
- Persistence?
- User activity?

## Useful Tools
- Registry Explorer
- RECmd
- RegRipper
- KAPE

## Important Caveats
Registry timestamps are **LastWrite** timestamps, not creation times.

## Common Keys
- Run Keys
- Services
- UserAssist
- MUICache
- TypedURLs
- RecentDocs
- USBSTOR
- MountedDevices
- ShellBags

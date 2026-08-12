# Windows Amcache Cheat Sheet

## Purpose
Amcache records metadata about executables and installed applications.

## Location
`C:\Windows\AppCompat\Programs\Amcache.hve`

## Evidence Provided
- Executable path
- SHA1 (first 31 MB)
- Compile timestamp
- File size
- Company
- Version
- Installation metadata

## Investigation Questions
- Was this executable present?
- Was malware installed?
- What version?
- What is its hash?

## Useful Tools
- AmcacheParser
- Registry Explorer
- RECmd
- KAPE

## Important Caveats
- May survive executable deletion.
- Does not always prove execution.
- Hash covers first 31 MB only.

## Example
Extract hash for `invoice.exe` and compare against threat intelligence.

## Quick Commands
`AmcacheParser.exe -f Amcache.hve`

# NTFS Master File Table (MFT) Cheat Sheet

## Purpose
The **Master File Table (MFT)** is the central metadata structure used by NTFS. Each file and directory is represented by an MFT record.

## Location
The NTFS metadata file is `$MFT`, normally at the root of an NTFS volume:

`C:\$MFT`

## Evidence Provided
- File and directory names
- File size and attributes
- MFT record number
- Parent directory reference
- File timestamps
- Allocation information
- Deleted-file indicators
- Alternate data stream information
- Resident content for some small files

## Important Attributes
### `$STANDARD_INFORMATION`
Contains standard file metadata, including timestamps and file attributes.

### `$FILE_NAME`
Contains filename/namespace information, parent-directory reference and another set of timestamp information.

Comparing these attributes can provide useful investigative context. Do not treat one timestamp as the complete story.

## Investigation Questions
- Did a file exist?
- What was its name and path?
- Which directory was it associated with?
- What metadata timestamps are recorded?
- Does the record indicate deletion?
- Are there multiple metadata records or names?

## What It Cannot Prove
MFT metadata alone does **not** prove:
- User intent
- Execution
- File opening
- Downloading
- Who modified a file
- The exact cause of a timestamp change

## Deleted Files
A deleted file's MFT record may remain until the record is reused. Metadata such as filename, size, timestamps and parent reference may therefore survive deletion.

## Resident vs Non-Resident Data
Small files may have content stored directly in the MFT record (**resident**). Larger files normally have data stored elsewhere (**non-resident**), with the MFT describing the data runs.

## Correlation
| Artifact | Useful for |
|---|---|
| USN Journal | File-system change activity |
| Prefetch | Execution evidence |
| Amcache | Executable/application metadata |
| Event Logs | System/security events |
| Registry | Configuration and persistence |
| Browser artifacts | Downloads/browsing activity |

## Practical Workflow
1. Record filename and path.
2. Record MFT record number.
3. Examine `$STANDARD_INFORMATION`.
4. Examine `$FILE_NAME`.
5. Compare timestamp sets.
6. Check for deletion.
7. Correlate with USN Journal.
8. Check Prefetch for execution.
9. Check Amcache for executable metadata.
10. Add relevant events to the timeline.

## Useful Tools
- MFTECmd
- KAPE
- Timeline Explorer

## Quick Commands
`MFTECmd.exe -f C:\Evidence\$MFT --csv C:\Output`

`MFTECmd.exe -d C:\Evidence --csv C:\Output`

## Important Caveats
- MFT timestamps are metadata, not a complete activity history.
- Timestamp behaviour can be affected by OS/application activity.
- MFT records can be reused.
- Deleted records can eventually be overwritten.
- Interpret timestamps according to their specific attribute and context.
- File presence in the MFT does not prove execution or user interaction.

## Key Takeaway
Think of the MFT as a **file-system metadata source**, not an activity log. Use it to establish what files/directories existed and their metadata, then correlate it with execution, Registry, journal, event-log and application artifacts.

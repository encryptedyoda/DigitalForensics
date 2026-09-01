# Windows USN Journal Cheat Sheet

## Purpose

The **USN Change Journal** records selected changes to files and directories on NTFS volumes.

For DFIR, it is useful for reconstructing file-system activity such as file creation, deletion, modification, and renames.

It complements the MFT: the MFT describes file-system metadata, while the USN Journal records selected changes.

## Location

The journal is associated with the NTFS volume and is commonly accessed as:

`$Extend\$UsnJrnl:$J`

## Evidence Provided

USN records can contain:

- File reference number
- Parent file reference number
- USN record number
- Timestamp
- Reason code(s)
- Source information
- Security ID
- Filename

## Important Reason Codes

| Reason | Meaning |
|---|---|
| `FILE_CREATE` | File or directory created |
| `FILE_DELETE` | File or directory deleted |
| `DATA_EXTEND` | File data was extended |
| `DATA_OVERWRITE` | File data was overwritten |
| `BASIC_INFO_CHANGE` | Basic file information changed |
| `RENAME_OLD_NAME` | Old name recorded during rename |
| `RENAME_NEW_NAME` | New name recorded during rename |

A single USN record can contain multiple reason flags.

## Investigation Questions

- Was a file created?
- Was a file deleted?
- Was a file renamed?
- When did the recorded change occur?
- What was the old or new filename?
- Which directory was involved?
- Can the activity be correlated with the MFT?

## What It Cannot Prove

A USN record does **not automatically prove**:

- Which person performed the action
- User intent
- That a file was executed
- That a file was opened by a user
- Why a file was renamed or deleted
- That every file-system action was recorded

Always correlate USN evidence with other artifacts.

## Rename Events

A rename commonly produces:

1. `RENAME_OLD_NAME`
2. `RENAME_NEW_NAME`

Do **not** conclude that two records are the same file merely because they appear consecutively in a parsed output.

Use identifying metadata and corroboration such as:

- File reference number
- Parent directory reference
- Sequence information
- Timestamps
- MFT records
- Other surrounding USN records

### Important DFIR Principle

For example, if you see:

`RENAME_OLD_NAME  invoice.exe`

followed by:

`RENAME_NEW_NAME  holiday.jpg`

the ordering is strong contextual evidence of a rename, but **adjacency alone is not a 100% identity guarantee**.

The relationship should be established from the record metadata and corroborated where possible.

## Journal Retention

The USN Journal has finite capacity. Older records can eventually be overwritten as the journal wraps.

Therefore:

> **Absence of a USN record does not prove that an event never occurred.**

## Correlation

| Artifact | Contribution |
|---|---|
| MFT | File metadata, names, timestamps, record identity |
| Prefetch | Execution evidence |
| Amcache | Executable/application metadata |
| Registry | Configuration and persistence |
| Event Logs | System/security events |
| Browser artifacts | Download/browsing evidence |
| Zone.Identifier | Mark-of-the-Web evidence |

### Example

USN:

`FILE_CREATE` for `invoice.exe`

MFT:

Corresponding file-system metadata exists.

Prefetch:

`INVOICE.EXE-*.pf` indicates execution.

Amcache:

Executable metadata and hash are recorded.

Together, these artifacts provide a much stronger timeline than any one artifact alone.

## Practical Investigation Workflow

1. Identify the filename.
2. Record the USN timestamp.
3. Record the reason code(s).
4. Record the file reference number.
5. Record the parent reference.
6. For renames, correlate the old/new name records using their metadata.
7. Pivot to the MFT.
8. Check Prefetch for execution.
9. Check Amcache for executable metadata.
10. Check Event Logs and other telemetry.
11. Build a timeline from corroborated evidence.

## Useful Tools

- MFTECmd
- KAPE
- Timeline analysis tools

MFTECmd can parse NTFS metadata including USN Journal records when the relevant evidence is available.

## Key Takeaway

The USN Journal is best thought of as a **record of selected NTFS change activity**.

It is excellent for questions such as:

**"Was this file created, renamed, modified, or deleted?"**

It is not a complete record of user activity.

Use it together with the **MFT, Prefetch, Amcache, Registry and Event Logs** to build a defensible forensic timeline.

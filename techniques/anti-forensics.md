# Anti-forensics seen in the wild

| Technique | Example | How it's still caught |
|-----------|---------|-----------------------|
| **Prefetch ACL tampering** | `icacls C:\Windows\Prefetch /deny user:(W)` → run tool → restore | Prefetch time gap, 4688/Sysmon cmdlines, $UsnJrnl ACL-change records |
| **Prefetch deletion / cleanup scripts** | Deleting `*.pf` after a session | $UsnJrnl `FileDelete`, $MFT orphan entries, `PfPerfStats.bin`/`Layout.ini` remnants |
| **COM self-destruct** | Temporary CLSID registered then deleted | Kernel/memory dump still holds hive pages; Sysmon 12/13; DCOM 10016 |
| **Timestamp stomping** | Future/odd PE `TimeDateStamp`; $SI times modified | $SI vs $FN comparison, $UsnJrnl, compile time vs first-seen (Amcache) |
| **Secure delete** | `shred -zu` (Linux) | bash history, filesystem journal, process accounting |
| **Event log clearing / bypass** | `wevtutil cl`, stopping EventLog service, thread-killing the service | EID 1102/104, the gap itself, EventLog service state, other log sources (SRUM, Prefetch) |
| **Recent items / jumplist wipe** | Deleting `Recent\*` | ShellBags, UserAssist, $UsnJrnl |
| **Spoofed install date** | Editing `InstallDate` to look like a fresh reset | Compare with `$MFT` of `C:\Windows`, setupapi logs, SYSTEM hive `LastWrite` times |

**Rule of thumb:** every wipe leaves a gap, and the gap usually lines up with the
event you're looking for.

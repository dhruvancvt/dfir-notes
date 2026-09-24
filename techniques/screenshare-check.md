# Screenshare / PC check — detection playbook

Defensive reference for anti-cheat screenshare (SS) checks: how to **detect** and
**prove** cheating so a ban decision is evidence-based. Detection only.

## Core questions and where the proof lives
| Question | Where to look | Proof |
|----------|---------------|-------|
| Was a program executed? | Prefetch, Amcache, BAM/DAM, UserAssist, 4688/Sysmon 1 | [forensic-artifacts](forensic-artifacts.md) |
| Is a DLL hijacked/sideloaded? | Duplicate system-named DLL in an app/writable dir | VirusTotal, DiE, triage sandbox — [dll-sideloading](dll-sideloading.md) |
| Was Prefetch tampered with? | `.pf` time gap, ACL change on `C:\Windows\Prefetch` | $UsnJrnl, 4688 cmdline (`icacls`) — [anti-forensics](anti-forensics.md) |
| Files with no name/extension? | `Get-ChildItem` filters, MFT, ADS enumeration | see below |
| Persistence set? | `RunOnce`/`Run`, `shell:startup`, PS profile | registry LastWrite, Amcache/Shimcache |
| Drive deleted/swapped to hide a cheat? | $UsnJrnl survives the volume; VSS snapshots; Shimcache | metadata mismatch vs the "clean" replacement |
| Was the event log evaded? | Log gaps, service state, EID 1102/104 | corroborate with SRUM/Prefetch that don't rely on the cleared log |

## Confirming a suspicious `.bat` / command
`icacls C:\Windows\Prefetch /deny *S-1-1-0:(W)` = deny **Write** to *Everyone*
on the Prefetch dir → blocks new `.pf` (anti-forensics). Confirm via the $UsnJrnl
ACL-change record and the 4688 command line.

## Finding nameless / extensionless files
```powershell
# no extension
Get-ChildItem -Recurse -Force | ? { $_.Extension -eq '' -and !$_.PSIsContainer }
# zero-byte
Get-ChildItem -Recurse -Force | ? { $_.Length -eq 0 -and !$_.PSIsContainer }
# alternate data streams
Get-Item * -Stream * | ? { $_.Stream -ne ':$DATA' }
```
Identify extensionless files by **magic bytes** (first 2–4 bytes), e.g. `MZ`
(4D 5A) = Windows PE, `%PDF` = PDF, `PK` = zip.

## "anydesk2.exe won't open while I'm connected"
A binary that refuses to run while AnyDesk/RDP is present is likely checking for
remote-session artifacts (anti-analysis). Next steps: pull it to a clean VM or
triage sandbox, VirusTotal, map behavior to MITRE ATT&CK. Don't clear it just
because it won't launch live.

## MZ / byte-flooding note (why static scanners miss things)
`MZ` is the DOS header magic on every Windows PE. Padding a binary with junk
bytes can push the real payload past the first N bytes some quick scanners read,
so use tools that scan the whole file, not just the header.

## Living-off-the-land
`winget`, `rundll32`, `msbuild`, `regsvr32`, etc. are legit but abusable (LOLBins,
see LOLBAS project). Monitor and log their use rather than trusting them.

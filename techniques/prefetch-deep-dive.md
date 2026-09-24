# Prefetch deep dive

Prefetch (`C:\Windows\Prefetch\*.pf`) exists to speed up app launches, and it
happens to be one of the best execution artifacts on Windows.

## What a .pf file gives you
| Field | Notes |
|-------|-------|
| Executable name | Uppercase, truncated to 29 chars |
| Path hash | The `-XXXXXXXX` suffix, a hash of the full path (and of the command line for hosts like `svchost`, `rundll32`, `dllhost`) |
| Run count | Total executions |
| Last run times | **Up to 8** on Win8+ (1 on Win7) |
| Volume info | Serial + creation time of the volume it ran from |
| Referenced files/dirs | Every file loaded in the **first ~10 seconds**, including DLLs, config files and the executable's own path |

## Key analysis moves
- **Same name, different hash:** `CHEAT.EXE-1A2B3C4D.pf` and `CHEAT.EXE-9F8E7D6C.pf`
  mean the same name ran from two different paths. A renamed cheat run from a USB or
  temp dir shows up this way.
- **Referenced files list:** shows the DLLs a process loaded, which exposes a
  sideloaded `winmm.dll` even after the DLL is deleted.
- **Volume serial:** ties execution to a specific (possibly now-removed) drive.
  Cross-reference with [USB history](usb-device-history.md).
- **File creation time ≈ first run** (minus ~10s). **Modified time ≈ last run.**

## Format quirks
- Win10/11 `.pf` files are **MAM-compressed** (header `MAM\x04`). They need a
  parser such as PECmd, WinPrefetchView or `libscca`, not a hex editor.
- Max ~1024 entries on Win10+. Old entries roll off.
- Prefetch can be **disabled** via
  `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters\EnablePrefetcher`
  (0 = off). A value of 0 on a gaming PC is itself suspicious; check the key's LastWrite time.
- SysMain (Superfetch) service must be running for Prefetch to update.

## Related files in the folder
- `Layout.ini`: file layout optimisation list, which survives some cleanups
- `PfPre_*.db`, `Ag*.db`: SysMain/Superfetch databases (harder to parse, but they hold history)
- `PfPerfStats.bin`: performance stats; its timestamps can contradict a wiped folder

## Tamper indicators
- A **time gap** in last-run times across all `.pf` files while the user was active
- Folder ACL modified (see [anti-forensics](anti-forensics.md))
- `EnablePrefetcher` changed recently
- $UsnJrnl shows `.pf` deletions

```powershell
# Quick triage: newest Prefetch activity
Get-ChildItem C:\Windows\Prefetch\*.pf | Sort LastWriteTime -Desc |
  Select -First 40 Name, CreationTime, LastWriteTime
# Tool: PECmd -d C:\Windows\Prefetch --csv out\
```

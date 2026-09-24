# AmCache & spoofed install date

## AmCache
`C:\Windows\AppCompat\Programs\Amcache.hve` — one of the strongest execution/
presence artifacts. Per binary it records **full path, SHA1, PE compile time,
and first-seen time**. Survives deletion of the original file. Parse with
AmcacheParser.

Use it to:
- Prove a now-deleted binary existed and when it first appeared.
- Get the SHA1 for VirusTotal without the file.
- Compare **PE compile time vs first-seen** — a compile time far in the future
  (or a first-seen long before the claimed install) is suspicious.

## Spoofed install / reset date
A "fresh reinstall" alibi can be checked, because the claimed `InstallDate`
rarely matches everything else:
- `SOFTWARE\Microsoft\Windows NT\CurrentVersion\InstallDate` vs
  the **$MFT creation time of `C:\Windows`** and `\Windows\System32`.
- setupapi logs, and registry-hive LastWrite times.
- Amcache first-seen entries **older** than the claimed install date prove the OS
  predates the story.
- A tiny amount of used space right after a "reset" (e.g. a near-empty drive) can
  itself be inconsistent with normal use — worth correlating, not conclusive alone.

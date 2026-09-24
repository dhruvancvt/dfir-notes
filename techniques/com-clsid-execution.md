# COM / CLSID execution

## How it's abused
- Register a class in `HKCU\Software\Classes\CLSID\{GUID}` (no admin needed) with
  `InprocServer32` (DLL) or `LocalServer32` (EXE).
- Trigger via `rundll32.exe -sta {GUID}`, `-localserver {GUID}`, `explorer shell:::{GUID}`,
  scripting (`GetObject`/`CreateObject`), or a hijacked CLSID that a legit app already loads.
- HKCU overrides HKLM, which makes HKCU a hijack point for existing CLSIDs.

## Proving execution
- **Prefetch** — `RUNDLL32.EXE-*.pf` run times; referenced files list the DLL.
- **4688 / Sysmon 1** — command line containing the GUID.
- **Sysmon 12/13/14** — key create/set under `\CLSID\`.
- **System log DCOM 10016 / 10010** — activation errors, often left behind when a server dies or its SID is gone.
- **Registry LastWrite** on the CLSID key, if it's still there.
- **Memory / kernel dump** — recovers keys the malware already deleted
  ([case](../cases/fivem-com-bypass.md)).

## Hunt
```powershell
Get-ChildItem HKCU:\Software\Classes\CLSID -ErrorAction SilentlyContinue |
  ForEach-Object { Get-ItemProperty "$($_.PSPath)\*Server32" -ErrorAction SilentlyContinue } |
  Select-Object PSParentPath, '(default)'
```

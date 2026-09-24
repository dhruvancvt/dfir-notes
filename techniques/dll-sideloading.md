# DLL sideloading / hijacking

A trusted (often signed) executable loads an attacker DLL because of search order.
The DLL sits in the app directory, or somewhere searched before `System32`.

## Common targets
- `winmm.dll`, `version.dll`, `dinput8.dll`, `d3d9.dll`/`dxgi.dll`, `api-ms-win-*`
- Game and launcher directories (writable by the user, and the processes are trusted)

## Detection
- **Sysmon EID 7** (ImageLoad) — system-named DLL loaded from a non-system path; unsigned.
- A DLL in an app directory **with the same name as a System32 DLL**.
- **Proxy DLLs** — many forwarded exports plus one extra thread (see [winmm loader](../cases/winmm-dll-loader.md)).
- **Timestamps** — $MFT creation time of the DLL vs the app install time. $SI vs $FN mismatch means timestomping.
- **Signature** — `sigcheck -a`, `Get-AuthenticodeSignature`.
- **Loaded modules** on a live box: `listdlls`, Process Explorer, System Informer.

## Quick hunt (PowerShell)
```powershell
$sys = (Get-ChildItem C:\Windows\System32\*.dll).Name
Get-ChildItem "C:\Program Files*","$env:LOCALAPPDATA" -Recurse -Filter *.dll -ErrorAction SilentlyContinue |
  Where-Object { $sys -contains $_.Name } |
  Select-Object FullName, CreationTime, @{n='Signed';e={(Get-AuthenticodeSignature $_.FullName).Status}}
```

## Related
Process hollowing — the DLL spawns a suspended legit process, unmaps its image,
and writes a payload in. Look for a mismatch between the image on disk and in memory
(e.g. `hollows_hunter`, `pe-sieve`).

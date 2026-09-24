# winmm.dll — "Dll4.dll" / hijackhostdll2 (stage-1 loader)

- **Type:** DLL hijack proxy loader, static RE
- **Verdict:** Malicious loader with C2 update loop
- **Attribution:** `de4f`, project name `hijackhostdll2`
- **Delivery:** see [winmm.dll drop chain](winmm-dll-drop-chain.md)

## Architecture — 3 stages
1. **Proxy** — exports ~180 Windows Multimedia (winmm) functions and forwards them
   to the real `System32\winmm.dll`, so the host process keeps working.
2. **Loader thread** — resolves its working directory and loads the stage-2
   payload ([region.dll](region-dll-wexize-revamp.md)).
3. **Update loop** — polls C2 over plain HTTP.

## C2 / decoded strings
| Item | Value |
|------|-------|
| C2 | `hxxp://3[.]133[.]88[.]x:8080` (AWS us-east-2) |
| Method | `GET` |
| Endpoints | `/get_true`/`/get_trust`, `/get_version` (partially decoded) |
| Auth param | `&hwid=` |
| Status strings | `banned`, `frozen`, `expired`, `success` |
| Staging dir | `%TEMP%\wuupd\` (masquerades as Windows Update) |

## Detection ideas
- `winmm.dll` anywhere outside `System32`/`SysWOW64`, especially in game or
  launcher directories.
- Export count ~180 with forwarders → proxy DLL.
- Directory `%TEMP%\wuupd\`.
- Outbound HTTP to :8080 from a game/launcher process.

## MITRE ATT&CK
T1574.001/.002 (DLL search order / sideloading) · T1071.001 (Web C2) · T1036 (Masquerading)

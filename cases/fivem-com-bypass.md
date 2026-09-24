# FiveM COM-server / RUNDLL32 bypass

- **Type:** Live PC-check forensics → ban report
- **Verdict:** Anti-cheat bypass confirmed

## Summary
An anti-cheat bypass that abuses Windows **COM** infrastructure: it registers a
temporary COM local server under custom GUIDs, launches via `rundll32.exe`, then
deletes its own registration (anti-forensics). The self-destruct was defeated by
taking a kernel live dump, which preserved the registry artifacts.

## Evidence / artifacts
- **RUNDLL32.EXE** executions referencing custom CLSIDs:
  - `722b3793-5367-4446-b6bb-db89b05c1f24`
  - `22d8c27b-47a1-48d1-ad08-7da7abd79617` (seen as `-localserver 22d8c27b-...`)
- **Registry** — COM class registration recovered from a kernel memory dump
  (spokwn's kernel dump tool) after the malware had deleted the on-disk keys.
- **Prefetch** (WinPrefetchView) — rundll32 run times.
- **Timeline correlation** — bypass at 23:18 → FiveM launch at 23:20.

## Technical findings
1. Bypass registers a COM class under `HKCU\Software\Classes\CLSID\{GUID}` with a
   `LocalServer32` / `InprocServer32` pointing at its payload.
2. Activation via `rundll32.exe` → COM runtime starts the "local server".
3. After injection, keys are removed so a registry check shows nothing.
4. Memory still holds the hive pages → live dump recovers them.

## Detection ideas
- Hunt `rundll32.exe` with `-localserver` / GUID arguments in Prefetch, 4688, Sysmon EID 1.
- Sysmon EID 12/13 on `\CLSID\{...}\LocalServer32` creation then deletion within minutes.
- Always take a memory/kernel dump **before** poking the registry on a live check.

## MITRE ATT&CK
T1218.011 (Rundll32) · T1546.015 (COM hijacking) · T1070 (Indicator removal)

## Output
Formal ban report (`.docx` + `.md`): executive summary, evidence, timeline, appendices.

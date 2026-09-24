# winmm.dll drop — malicious command chain

- **Type:** Command-line / delivery analysis
- **Verdict:** Malicious — sideloading with Prefetch anti-forensics

## The chain
1. **Blind Prefetch** — `icacls C:\Windows\Prefetch /deny <user>:(W)` stops new
   `.pf` files being written, so the next steps leave no Prefetch trace.
2. **Download** — `curl` pulls the DLL from the **Discord CDN**
   (`cdn.discordapp[.]com/attachments/...`).
3. **Sideload** — saves it as `winmm.dll` in the Epic Games Launcher `Win64`
   directory. The signed launcher loads it via DLL search order.
4. **Cover up** — restores the Prefetch ACLs so the folder looks normal.

## What still catches it
- **Prefetch gap** — no `.pf` created/updated during the window even though
  apps ran.
- **Security 4670 / object-access** (if auditing is on) — permission change on Prefetch.
- **4688 / Sysmon 1** — `icacls` and `curl` command lines.
- **$UsnJrnl / $MFT** — creation of `winmm.dll` in the launcher dir; ACL change on Prefetch.
- **Zone.Identifier ADS** — may be missing with curl, so its absence proves nothing.
- **SRUM** — network bytes for `curl.exe`.

## MITRE ATT&CK
T1222.001 (Permissions modification) · T1105 (Ingress tool transfer) ·
T1574.002 (DLL sideloading) · T1070 (Indicator removal)

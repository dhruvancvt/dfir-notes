# PowerShell `-NoProfile` execution

`powershell -nop -ep bypass ...` skips profile scripts, so profile-based
logging or monitoring never runs. It does **not** bypass:

- **Microsoft-Windows-PowerShell/Operational**: 4103 (module logging),
  4104 (script block logging, auto-logs "suspicious" blocks even when not enabled), 4100–4106
- **Security 4688** with command-line auditing
- **Sysmon EID 1**
- **Prefetch** — `POWERSHELL.EXE-*.pf`
- **PSReadLine history** — `%APPDATA%\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`
  (interactive sessions only)

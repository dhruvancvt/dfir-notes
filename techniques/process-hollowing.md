# Process hollowing / injection — detection

A benign process is started suspended, its image is unmapped, and a malicious
image is written in its place, so the process name looks legit but the running
code isn't.

## Indicators
- **Image on disk ≠ image in memory** — the primary tell.
- Memory regions that are private + executable (RWX) where the mapped image
  should be.
- A process whose path/parent doesn't match its name (e.g. a `svchost.exe` not
  spawned by `services.exe`, or running from a user dir).
- `CreateProcess` with `CREATE_SUSPENDED`, then `NtUnmapViewOfSection` /
  `WriteProcessMemory` / `SetThreadContext` / `ResumeThread` in Sysmon or an EDR trace.

## Tools
- **pe-sieve / hollows_hunter** (hasherezade) — scans live processes for hollowing,
  injected PE, and patched code; dumps the suspect regions.
- Process Explorer / System Informer — compare image path, verify signatures,
  inspect memory.
- Volatility (`malfind`, `ldrmodules`) on a memory dump.

## Sysmon
EID 1 (process create — check parent/path), EID 8 (CreateRemoteThread),
EID 10 (ProcessAccess with suspicious granted rights on lsass/other targets).

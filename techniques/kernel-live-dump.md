# Kernel / live memory dump for live checks

Capturing volatile state before anti-forensics can erase on-disk traces (the
technique that recovered the deleted COM keys in the [FiveM COM case](../cases/fivem-com-bypass.md)).

## Why first
Registry keys, injected modules and unlinked processes may exist only in memory
after a sample self-deletes. Dump **before** you start browsing the registry.

## Options
- **Full RAM capture** — WinPKG/DumpIt, Magnet RAM Capture, Belkasoft; analyze with Volatility.
- **Kernel live dump** — Windows feature capturing kernel memory without a crash;
  triggered via WinDbg (`.dump /ka`), NotMyFault, or a live-dump helper.
- **Process dumps** — System Informer / Process Explorer, right-click → Create dump
  (user-mode; good for a specific process, not full kernel memory).

System Informer creates user-mode process dumps and views kernel memory, but for
a true full **kernel** dump use WinDbg or the Windows live-dump mechanism.

## Analyze
Volatility / Rekall: process list, unlinked processes, injected code, loaded
modules, registry hives resident in memory, network connections.

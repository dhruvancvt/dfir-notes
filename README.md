# DFIR Notes

Personal knowledge base of digital forensics, malware analysis, and anti-cheat
forensics work — mostly Windows, with a focus on FiveM cheat/bypass detection,
loader reverse engineering, and live PC checks.

> All case material is sanitized: suspect hostnames, usernames, and personal
> data are removed. IOCs are **defanged** (`hxxp`, `[.]`) — do not re-fang
> and visit them outside an isolated lab.

## Contents

### Cases — real-world analysis
| Case | Type | Verdict |
|------|------|---------|
| [FiveM COM-server / RUNDLL32 bypass](cases/fivem-com-bypass.md) | Live PC check | Bypass confirmed |
| [region.dll — "Wexize Revamp"](cases/region-dll-wexize-revamp.md) | Stage-2 payload RE | Cheat |
| [winmm.dll — hijackhostdll2 loader](cases/winmm-dll-loader.md) | Stage-1 loader RE | Malicious loader |
| [winmm.dll drop — command chain](cases/winmm-dll-drop-chain.md) | Delivery analysis | Malicious |
| [Pyrex BP — live check](cases/pyrex-bp-live-check.md) | Live forensics | Suspicious gap |
| [Vex.exe](cases/vex-exe.md) | Cheat RE | Cheat (KeyAuth) |
| [Infinity External](cases/infinity-external.md) | Full PE analysis | Cheat |
| [Renaissance](cases/renaissance.md) | Triage | **Not** a stealer |
| [SneakyKeys keylogger + PCAP](cases/sneakykeys.md) | RE + network decrypt | Keylogger |
| [AD1 evidence image](cases/ad1-evidence-image.md) | Image analysis | — |
| [ApiSet stub DLL](cases/apiset-stub-dll.md) | Static analysis | Clean (timestomped) |

### Techniques & references
- [Screenshare / PC check playbook](techniques/screenshare-check.md)
- [DLL sideloading / hijacking](techniques/dll-sideloading.md)
- [Process hollowing / injection detection](techniques/process-hollowing.md)
- [Anti-forensics](techniques/anti-forensics.md)
- [COM / CLSID execution](techniques/com-clsid-execution.md)
- [Forensic artifacts — proving execution](techniques/forensic-artifacts.md)
- [Prefetch deep dive](techniques/prefetch-deep-dive.md)
- [AmCache & spoofed install date](techniques/amcache-install-date.md)
- [USB device history](techniques/usb-device-history.md)
- [DMA cheat-device detection](techniques/dma-card-detection.md)
- [Kernel / live memory dump](techniques/kernel-live-dump.md)
- [PowerShell no-profile execution](techniques/powershell-noprofile.md)
- [Streamproof overlays](techniques/streamproof-overlays.md)

### CTF — HTB Sherlocks
- [Brutus](ctf/htb-sherlocks/brutus.md) · [An Unusual Sighting](ctf/htb-sherlocks/an-unusual-sighting.md) · [Phreaky](ctf/htb-sherlocks/phreaky.md) · [Enigma](ctf/htb-sherlocks/enigma.md)

### Detection & intel
- [Detections](detections/README.md) — YARA, IOCs
- [MISP vs OpenCTI vs Neo4j](platforms/misp-opencti-neo4j.md)
- [Tooling](tooling/notes.md)

### Reference
- [MITRE ATT&CK index](mitre-attack-index.md): techniques across all cases
- [Glossary](glossary.md)
- [Resources](resources.md): tools, references, learning

## About
- [Disclaimer](DISCLAIMER.md) · [License (CC BY 4.0)](LICENSE.md) · [Contributing](CONTRIBUTING.md)
- Case write-up template: [`cases/_TEMPLATE.md`](cases/_TEMPLATE.md)

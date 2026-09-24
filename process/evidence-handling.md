# Evidence handling & chain of custody

## Principles
- **Integrity:** Hash evidence at acquisition (SHA-256; MD5 + SHA-1 are still
  common for tool compatibility). Re-hash before analysis and before reporting.
- **Work on copies:** Never analyze the original. Mount images read-only.
- **Write blocking:** Hardware write blockers for physical drives; read-only
  mounts for images.
- **Document:** Who, what, when, where, how, for every transfer and action.

## Chain of custody record
| Field | Example |
|-------|---------|
| Evidence ID | CASE-001-E01 |
| Description | 512 GB NVMe, serial ..., from workstation WS-XX |
| Acquired by / date (UTC) | Analyst name, 2026-01-01 14:02 |
| Acquisition method | FTK Imager, E01, verified |
| Hashes | SHA-256: ... |
| Storage location | Evidence locker / encrypted share |
| Transfers | Date, from, to, purpose, signature |

## Image formats
| Format | Notes |
|--------|-------|
| RAW / dd | Bit-for-bit, no metadata, universal |
| E01 (EWF) | Compressed, embedded hashes and case metadata, widely supported |
| AFF4 | Open, supports large images and memory |
| AD1 | AccessData logical image (files, not full disk), FTK Imager only |
| VMDK / VHDX | Virtual disks, can often be mounted directly |

## Live vs dead-box acquisition
- **Live:** Needed for memory, encrypted volumes that are currently unlocked,
  cloud-only systems, and servers that can't go down. Every action changes the
  system, so document commands run and their times.
- **Dead-box:** Cleanest for disk, but volatile data is lost and full-disk
  encryption may lock you out.
- Check for BitLocker or other encryption **before** pulling power
  (`manage-bde -status`). Get the recovery key if you can.

## Triage collection
When full imaging isn't practical, collect the high-value artifacts only:
- **KAPE** targets (e.g. `!SANS_Triage`, `KapeTriage`) plus modules to parse.
- **Velociraptor** offline collector or live hunts across many hosts.
- Always record the collector version and config used.

# Windows artifacts — proving execution & activity

## Execution
| Artifact | Path | Proves | Notes |
|----------|------|--------|-------|
| Prefetch | `C:\Windows\Prefetch` | exec, count, last 8 runs, loaded files | disabled on some SSD configs; tool: PECmd, WinPrefetchView |
| PfPerfStats.bin / Layout.ini | `C:\Windows\Prefetch` | Prefetch/SysMain internals | survives some cleanups |
| Amcache | `AppCompat\Programs\Amcache.hve` | presence + SHA1, first-seen | AmcacheParser |
| ShimCache | SYSTEM `AppCompatCache` | existence | not proof of exec on Win10+ |
| BAM/DAM | SYSTEM `bam\State\UserSettings` | last exec per SID | cleared on some updates |
| UserAssist | NTUSER `UserAssist` | GUI launches | ROT13 names |
| SRUM | `System32\sru\SRUDB.dat` | per-app CPU/network, 30–60 days | srum-dump |
| Event logs | 4688, Sysmon 1, PS 4103/4104 | exec + cmdline | if enabled |
| Jumplists / LNK | `Recent\` | file/app opened | LECmd, JLECmd |

## Filesystem
- **$MFT** — $SI vs $FN timestamps (timestomping), deleted entries. MFTECmd.
- **$UsnJrnl:$J** — create/delete/rename/ACL-change timeline.
- **$LogFile** — short-term metadata changes.
- **Zone.Identifier ADS** — download origin (browsers set it; curl doesn't).

## Devices
- **USB:** `SYSTEM\...\Enum\USBSTOR`, `Enum\USB`, `MountedDevices`,
  `setupapi.dev.log`, Partition/Diagnostic EID 1006, DriverFrameworks EIDs 2003/2100.
- **DMA cards:** check PCIe device list for spoofed VID/PID (FPGA boards that
  pretend to be NICs, etc.), IOMMU/Kernel DMA Protection status, device install
  times in setupapi.

## Screenshare check order (live)
1. Memory/kernel dump first. It's volatile and beats the anti-forensics.
2. Prefetch and timeline gaps.
3. Amcache / BAM / UserAssist for tools that were run.
4. Event logs (look for clears and gaps).
5. $UsnJrnl for deletions and renames.
6. USB / device history.

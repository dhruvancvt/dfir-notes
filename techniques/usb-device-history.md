# USB device history — detecting removed storage

Proving a USB mass-storage device was connected, even after it's unplugged.

## Registry (historical)
- `SYSTEM\CurrentControlSet\Enum\USBSTOR` — vendor/product, serial, first/last connect
- `SYSTEM\...\Enum\USB` — device instances
- `SYSTEM\MountedDevices` — drive-letter ↔ device mapping
- `SOFTWARE\Microsoft\Windows Portable Devices\Devices` — friendly names
- NTUSER.DAT hives + VSS copies preserve older states

## Event logs
```powershell
Get-WinEvent -FilterHashtable @{LogName='System'; ID=20001,20003} |
  ? { $_.Message -like '*USB*' }
```
- System 20001 (device install) / 20003 (removal)
- Microsoft-Windows-Kernel-PnP/Configuration
- Partition/Diagnostic EID 1006; DriverFrameworks-UserMode 2003/2100
- setupapi.dev.log — first install timestamp

## Filesystem artifacts
- LNK files in `Recent\` pointing at the drive letter
- ShellBags — folders browsed on the device
- `Windows.edb` search index; Thumbs.db references

## Tools
RegRipper, USB Detective, FTK Imager, MFTECmd, Eric Zimmerman's registry tools.

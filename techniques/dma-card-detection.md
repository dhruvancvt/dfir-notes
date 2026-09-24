# DMA cheat-device detection

Detecting a DMA/FPGA card that spoofs its PCIe identity (e.g. reports itself as a
NIC) to read game memory. Detection only.

## Hardware / firmware level
- **PCIe enumeration** — compare the device tree against a known-good baseline;
  look for odd vendor/device IDs or capability structures that don't match a real
  card of that class.
- **IOMMU / VT-d** — enable Kernel DMA Protection; inspect IOMMU groups for
  devices that appear/disappear or map unexpected memory ranges.
- Spoofed cards often copy a legit NIC's VID/PID but get config-space details or
  BAR layout subtly wrong.

## Software checks
```powershell
# PCIe devices
Get-WmiObject Win32_PnPEntity | ? { $_.DeviceID -like 'PCI\*' } |
  Select Name, DeviceID, Manufacturer | Sort Name
# recently installed PCI devices
Get-WinEvent -FilterHashtable @{LogName='System'; ID=20001,20002,20003}
```
- Driver Verifier for unsigned/suspicious drivers.
- Watch for systematic memory-scan patterns (anti-cheat kernel telemetry).

## Notes
Modern kernel anti-cheats fingerprint hardware and monitor DMA patterns; a clean
PCIe baseline plus Kernel DMA Protection is the practical defensive posture.

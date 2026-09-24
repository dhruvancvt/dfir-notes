# Streamproof overlays

Cheat overlays that stay invisible to screenshots, OBS, Discord streams and screenshare tools.

| Method | Mechanism | Detect |
|--------|-----------|--------|
| `SetWindowDisplayAffinity(WDA_EXCLUDEFROMCAPTURE)` | DWM omits the window from capture | Import in non-system module; enumerate windows and call `GetWindowDisplayAffinity` |
| NVIDIA overlay / NvFBC abuse | Draws in, or impersonates, NVIDIA's capture path | Unsigned `NvFBC64.dll` outside the NVIDIA dir; signature check |
| Hijacked legit overlay windows | Draws inside Discord/Steam/NVIDIA overlay windows | Overlay process with unexpected modules |

Seen in: [region.dll](../cases/region-dll-wexize-revamp.md), [Vex](../cases/vex-exe.md), [Infinity](../cases/infinity-external.md).

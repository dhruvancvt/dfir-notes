# region.dll — "Wexize Revamp"

- **Type:** Stage-2 payload static RE
- **Verdict:** Full-featured FiveM cheat
- **Attribution:** developer handle `de4f`, PDB path ends `\Wexize Revamp.pdb`
- **Loaded by:** [winmm.dll hijack loader](winmm-dll-loader.md)

## Technical findings
- **Libraries (statically linked):** Dear ImGui (overlay), libcurl 7.70.0,
  nlohmann/json 3.11.3, FreeType, DirectX 9/11 rendering.
- **Memory access:** imports `ZwReadVirtualMemory` / `ZwWriteVirtualMemory` —
  NT layer, skipping the Win32 `ReadProcessMemory` wrappers that are commonly hooked.
- Loads `citizen-playernames-five.dll` (FiveM component) for player data.
- **Features (from strings/UI):** aimbot (bone targeting, smoothing), silent aim,
  triggerbot, player/vehicle ESP with skeletons, GodMode, NoClip, infinite ammo,
  vehicle handling editor, teleports, player list.

## Streamproof
- `SetWindowDisplayAffinity(hwnd, WDA_EXCLUDEFROMCAPTURE)` hides the overlay from
  screenshots / OBS / screenshare.
- NVIDIA streamproof mode — overlay hidden under NVIDIA capture.
See [streamproof overlays](../techniques/streamproof-overlays.md).

## Related clean verdict — nvcontainer.exe
Found alongside; confirmed **legitimate** NVIDIA binary: valid Authenticode chain
(DigiCert), clean import table, expected path. Good reminder to verify, not assume.

## Detection ideas
- PDB strings `Wexize`, handle `de4f`.
- Unsigned DLL importing both ImGui symbols and `Zw*VirtualMemory`.
- `SetWindowDisplayAffinity` import in a non-system DLL loaded into a game.

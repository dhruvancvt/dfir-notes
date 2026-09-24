# Infinity External (Infinity.exe + NvFBC64.dll)

- **Type:** Full PE analysis across two builds → PDF report
- **Verdict:** External cheat with layered streamproofing
- **Deliverables:** PDF report, 3 YARA rules, MISP IOC JSON, auth-host recovery harness

## Findings
- **auth.dat** — DPAPI-protected credential/licence blob (`CryptProtectData`),
  tied to the user profile.
- **D3D11 overlay.**
- **Two-layer streamproof:**
  1. `SetWindowDisplayAffinity` exclusion
  2. `NvFBC64.dll` — abuses or impersonates NVIDIA's Frame Buffer Capture library,
     so capture tools see a clean frame
- **Clip recorder** built in.

## Auth-host recovery harness
The auth host was not in plaintext, so three approaches were used:
- **Frida** — hook the resolver/HTTP calls at runtime.
- **x64dbg** — break on decryption routine return.
- **Unicorn** — emulate the string-decrypt routine offline, without running the sample.

## Detections
YARA rules live in [`detections/yara/`](../detections/yara/). IOCs are in [`detections/iocs/`](../detections/iocs/).

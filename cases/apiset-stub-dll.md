# ApiSet stub DLL (api-ms-win-core-debug-l1-1-0.dll)

- **Type:** Static analysis
- **Verdict:** **Clean**. Legitimate ApiSet forwarder, with a timestamp anomaly.

## Findings
- 4 exports, all forwarders to `kernel32`. No code of its own.
- Signed, **Epic Games** signer (redistributed with the launcher), Win11 22H2 version metadata.
- **Anomaly:** PE `TimeDateStamp` = `0xBE4C490B` → **4 Mar 2071**. Most likely
  timestamp stomping, or a reproducible-build hash in place of a real timestamp.
  Microsoft reproducible builds do this, so it isn't automatically malicious.

## Why it was checked
`api-ms-win-*` names are commonly abused for **sideloading** because they look
like system files. The things to verify:
1. Are all exports forwarders? (A real stub has no meaningful code.)
2. Is the signature valid, with the expected signer?
3. Does the file size match known-good copies?

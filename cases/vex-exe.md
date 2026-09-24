# Vex.exe

- **Type:** Cheat binary static RE → redacted PDF report
- **Verdict:** Cheat using KeyAuth licensing

## Findings
- **KeyAuth** — commercial auth/licensing SaaS. Identified from its API strings
  and request patterns.
- **"Cracked" build:** auth is bypassed with **4 patched conditional jumps** plus
  a **fake local Python auth server** that answers KeyAuth-style requests.
- **StreamProof overlay** — hidden from capture ([details](../techniques/streamproof-overlays.md)).

## Notes
- Cracked cheats often carry extra payloads. A patched binary with a local
  server needs checking for stealer behavior too.
- Diff the patched binary against the original build to find the jump patches.

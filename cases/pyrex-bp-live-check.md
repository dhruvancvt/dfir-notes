# Pyrex BP — live PC check

- **Type:** Live forensics → `.md` report
- **Verdict:** Suspicious — evidence of execution during a blind window
- *(Suspect host/user identifiers redacted.)*

## Findings
- **System log — DCOM Event ID 10016** with an unavailable container SID. Points
  to a COM activation from a context that no longer exists, consistent with
  the [COM-based bypass family](fivem-com-bypass.md).
- **~10 minute Prefetch gap** — no `.pf` updates during a window when the user
  was clearly active. That points to Prefetch being blocked or wiped (see
  [anti-forensics](../techniques/anti-forensics.md)).

## Method
1. Snapshot Prefetch last-run times → sort → look for holes.
2. Line up the holes with event logs (System 10016, Security 4624/4688) and the FiveM launch time.
3. Report the gap + the DCOM event as corroborating indicators.

## Lesson
Missing evidence is evidence. A gap in an artifact that normally updates
constantly is a finding in itself.

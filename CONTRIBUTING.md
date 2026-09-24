# Contributing

Corrections and additions are welcome.

## Adding a case
1. Copy [`cases/_TEMPLATE.md`](cases/_TEMPLATE.md) to `cases/<short-name>.md`.
2. Fill in the summary, evidence, findings, IOCs, ATT&CK mapping and lessons.
3. Add a row to the case table in the [README](README.md) and, if needed, to the
   [ATT&CK index](mitre-attack-index.md).

## Rules
- **Sanitize:** no real usernames, hostnames, SIDs, emails, passwords or tokens.
- **Defang IOCs:** `hxxp://`, `example[.]com`, `1[.]2[.]3[.]4`.
- **No live samples** in the repo. Use hashes only.
- **No CTF flags** (HTB's rules forbid publishing them).
- Keep the focus on detection and analysis.

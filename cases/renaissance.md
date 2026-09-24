# Renaissance

- **Type:** Triage / verdict
- **Verdict:** **Not a credential stealer / token logger**

## Question
Does `renaissance.exe` log Discord tokens or steal credentials?

## Checks performed
- Strings / imports for Discord token paths
  (`%APPDATA%\discord\Local Storage\leveldb`, `Login Data`, `Local State`),
  webhook URLs (`discord[.]com/api/webhooks`), browser DB access, DPAPI decryption of browser keys.
- Network destinations.

## Findings
- Only network use is **HWID-based licence auth** to `yusei[.]cc`.
- No token/browser-credential harvesting logic, no webhook exfil.

## Why this matters
Clearing a sample takes as much rigor as convicting one. Write down what you
checked, not only what you found.

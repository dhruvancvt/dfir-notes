# SneakyKeys.exe + cap.pcapng — keylogger with IRC C2

- **Type:** Full RE + network decryption (threat-intel work sample, lab environment)
- **Verdict:** Keylogger exfiltrating over ChaCha20-encrypted IRC

## Crypto
- **ChaCha20-256** (original DJB variant, 64-bit nonce).
  - Confirmed by the sigma constant `expand 32-byte k` and a `chacha20.h` artifact in `.rdata`.
- **Static key** `My_dUp3r_sup3r_kon3_n0nc` at `.data:0x1400d4000`, nonce `on3_n0nc` at +16.
  The layout is sloppy and overlapping: the key over-reads into the port bytes `0x29 0x1a` (= 6697).
- The static key is used for string/command obfuscation.

## Network
- Raw TCP/IPv4 via Winsock (`AF_INET`, `SOCK_STREAM`), **IRC** at the application layer.
- Bot nick `ALICE_<8 hex>` (UUID-derived), channel `#key_storrage`, server `192.168.1.80:6697` (lab).
- Keystrokes sent as hex-encoded ChaCha20 ciphertext in `PRIVMSG`.

## Gotcha — runtime key ≠ static key
Traffic did **not** decrypt with the static key. The network path uses the
**session UUID** (from the host `MachineGuid`, dashes removed) as the key, with
the same nonce and **counter reset per message**. After switching keys the
PCAP decrypted fully; the victim's plaintext keystrokes included a WordPress
credential (redacted).

## Lessons
- Find every call site of the cipher, not just the first key you spot.
- A per-message counter reset means keystream reuse, which is weak crypto worth noting in the report.

## MITRE ATT&CK
T1056.001 (Keylogging) · T1071 (App-layer protocol: IRC) · T1573.001 (Symmetric encryption)

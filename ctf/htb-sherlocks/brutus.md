# HTB Sherlock — Brutus

**Evidence:** `auth.log` + binary `wtmp`.

## Findings
- SSH brute force → successful **root** login.
- Parsed `wtmp` with a utmp parser to pin down the interactive session times.
- systemd-logind **session 37** = attacker's interactive session.
- Persistence: new user **`cyberjunkie`** (UID 1002) added to sudo.
- Downloaded `linper.sh` (a Linux persistence toolkit) via `sudo curl`.

## MITRE ATT&CK
T1110 (Brute force) · T1078 (Valid accounts) · T1136.001 (Local account) ·
T1003.008 (/etc/passwd & shadow) · T1105 (Ingress tool transfer)

## Hardening
Key-only SSH, fail2ban, `PermitRootLogin no`, alerting on new sudoers.

# HTB Sherlock — An Unusual Sighting

**Evidence:** SSH `auth.log`, bash history.

## Findings
- The legit admin sessions came over Tailscale. The attacker's didn't: an
  off-hours root login from an unfamiliar external IP.
- Kill chain: `/etc/shadow` dump → payload pulled from a **typosquatted** domain
  (`gnu-packages[.]com`) → `./setup` run → `shred -zu` to destroy evidence.

## Lesson
Baseline normal access first (here, Tailscale). Anomalies against the baseline
stand out right away.

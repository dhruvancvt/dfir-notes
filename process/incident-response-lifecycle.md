# Incident response lifecycle

Based on NIST SP 800-61 (Preparation → Detection & Analysis → Containment,
Eradication & Recovery → Post-Incident Activity), with practical notes.

## 1. Preparation
- Asset inventory and network map: you can't scope what you don't know exists.
- Logging baseline: Sysmon, PowerShell script-block logging, command-line
  auditing (4688), centralized collection (SIEM or at least forwarded EVTX).
- Tooling staged in advance: triage collector (KAPE, Velociraptor), imaging
  tools, clean analysis VM, write blockers.
- Contact list, escalation paths, legal/HR contacts, out-of-band comms channel.
- Playbooks per scenario (ransomware, BEC, insider, malware on endpoint).

## 2. Detection & analysis
- **Triage:** Is this real? What's affected? How bad?
- **Scope:** Find every affected host and account. Pivot on IOCs (hashes, domains,
  IPs, filenames, usernames) across the whole fleet.
- **Timeline:** Build a single ordered list of events from all sources.
- **Classify severity:** Data exposure, business impact, spread.
- Document everything with timestamps (UTC) from minute one.

## 3. Containment
- **Short-term:** Isolate hosts (EDR network containment, pull from VLAN),
  disable compromised accounts, block IOCs at the perimeter.
- **Preserve before you change:** Capture memory and triage artifacts
  before rebooting or reimaging.
- **Long-term:** Temporary fixes that keep the business running while you clean up.

## 4. Eradication
- Remove malware, persistence mechanisms, rogue accounts.
- Reset credentials (including service accounts, and krbtgt twice for AD compromise).
- Patch the entry point.

## 5. Recovery
- Restore from known-good backups, rebuild where trust is lost.
- Heightened monitoring on restored systems for re-infection.
- Staged return to production.

## 6. Post-incident
- Blameless lessons-learned meeting within ~2 weeks.
- Root cause, what detection worked, what didn't, time-to-detect / time-to-contain.
- Turn findings into new detections, hardening tasks, playbook updates.

## Order of volatility (RFC 3227)
Collect most volatile first:
1. CPU registers, cache
2. Memory (RAM), running processes, network connections
3. Temporary filesystems
4. Disk
5. Remote logs and monitoring data
6. Physical configuration, network topology
7. Archival media

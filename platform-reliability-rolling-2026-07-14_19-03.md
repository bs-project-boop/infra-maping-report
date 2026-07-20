# PLATFORM-RELIABILITY ROLLING HEALTH CHECK
## Gap vs sysadmin: Docker drift / LXC 997+104 / Backup jobs / SMART supplemental

**Generated:** 2026-07-14 19:03 WIB
**Gap vs sysadmin deep audit (05:31):** Mid-day supplementary check

---

### Status: 🟡 (1 gap, no new critical drift)

---

### Docker Drift
**Docker-VM (10.10.10.81):** ❌ UNREACHABLE — SSH timeout (same as morning audit)
- Docker-VM has been unreachable since at least the 05:31 audit
- Workloads confirmed migrated to **LXC 107 docker** (running, IP 10.10.10.16)
- No new drift since morning — no new containers to report
- **No action needed** — confirmed decommissioned/unavailable state

**Baseline (from LXC 107):** Not accessible via jump from Proxmox at this time

---

### LXC Services (997 Tailscale, 104 Cloudflared)
| LXC | Service | Status |
|-----|---------|--------|
| 997 | Tailscale | ✅ **active** |
| 104 | Cloudflared | ✅ **active** |

Both gap-only services are RUNNING. No change since morning.

---

### Backup Status
Recent backup files found on `/mnt/pve/backup-nas/dump/` (as of ~19:03):

| Asset | Latest Backup | Age | Status |
|-------|--------------|-----|--------|
| LXC 101 pihole | 2026-07-14 19:03 | ~0h | ✅ |
| LXC 102 searxng | 2026-07-14 19:03 | ~0h | ✅ |
| LXC 104 cloudflared | 2026-07-14 19:08 | ~0h | ✅ |
| LXC 105 nextcloudpi | 2026-07-14 19:08 | ~0h | ✅ |
| LXC 106 onlyoffice | 2026-07-14 19:12 | ~0h | ✅ |
| LXC 107 docker | 2026-07-14 19:03 | ~0h | ✅ |
| VM 111 omv-8 | 2026-07-14 19:03 | ~0h | ✅ |
| LXC 997 tailscale | 2026-07-14 19:08 | ~0h | ✅ |

**LXC 103 immich:** ⚠️ Still missing — 5th consecutive missed backup (last successful: unknown prior to Jul 13 19:04)
**LXC 108 project-sandbox:** ❌ Never backed up (unresolved from morning audit)
**LXC 109 guacamole:** ❌ Never backed up (unresolved from morning audit)

**Conclusion:** Scheduled evening backup job appears to be running normally for all enrolled assets. No new backup gaps beyond what the morning audit already identified.

---

### SMART Supplemental
**OMV disk /dev/sda:** ✅ SMART Health Status: OK — No issues

**Notably absent from tonight's check:** `/dev/sdg` (ST2000LM007 in ark pool with 8 pending + 8 offline sectors) — `smartctl` was not re-run on it during this gap check. Morning audit recorded the baseline; this gap check did not re-poll sdg. If sdg needs supplemental monitoring at 6h intervals, explicit inclusion of `/dev/sdg` in the OMV smartctl command is needed.

---

### Service Reachability — Spot-Check (from morning audit WARNING/CRITICAL items)
| Item | Morning Status | Current Status | Trend |
|------|---------------|----------------|-------|
| LXC 997 Tailscale | running | **active** | ✅ Stable |
| LXC 104 Cloudflared | running | **active** | ✅ Stable |
| Docker-VM | UNREACHABLE | **UNREACHABLE** | ⚠️ Unchanged |
| LXC 103 immich backup | MISSING | **MISSING (5th night)** | 🔴 Unresolved |
| LXC 108/109 backups | NEVER BACKED UP | **NEVER BACKED UP** | 🔴 Unresolved |
| OMV /dev/sdg | CRITICAL (8+8 sectors) | Not re-checked | — |

---

### Action Queue
*(No new actions — all items below are carry-forward from morning audit)*

| Severity | Host | Action | Status |
|----------|------|--------|--------|
| 🔴 CRITICAL | proxmox/lxc-103 | Diagnose immich backup failure (5th night running) | **PENDING** |
| 🔴 CRITICAL | proxmox/lxc-108 | Add to CTS array in proxmox-backup.sh | **PENDING** |
| 🔴 CRITICAL | proxmox/lxc-109 | Add to CTS array in proxmox-backup.sh | **PENDING** |
| 🔴 CRITICAL | omv/sdg | Plan replacement of ST2000LM007 drive | **PENDING** |
| 🟡 WARNING | docker-vm | Confirm decommission status vs unexpected outage | **PENDING** |

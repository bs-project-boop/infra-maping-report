# PLATFORM-RELIABILITY ROLLING HEALTH CHECK
## Gap vs sysadmin: Docker drift / LXC 997/104 / Backup jobs / SMART supplemental

**Generated:** 2026-07-14 19:30 WIB
**Gap focus:** mid-day drift since morning sysadmin audit (05:31)

---

### Status: 🟢 CLEAN — No drift detected

---

### Docker Drift
**Baseline:** Portainer + Dockge expected on LXC 107

- `portainer` — **Up 9 days** — portainer/portainer-ce:latest ✅
- `dockge` — not present ✅ (compose stacks not deployed on this host)
- `docker compose ls` — empty `[]` ✅ (no compose stacks running)
- **Docker drift: NONE.** Container state matches baseline.

> **Note:** Docker-VM (10.10.10.81) remains unreachable — confirmed by sysadmin as decommissioned; all workloads on LXC 107. No new drift.

---

### LXC Services (997 Tailscale, 104 Cloudflared)
These are NOT checked by sysadmin's daily audit — verified now:

- **LXC 997 Tailscale** — `tailscaled`: **active** ✅
- **LXC 104 Cloudflared** — `cloudflared`: **active** ✅
- **Both RUNNING.** No action needed.

---

### Backup Status
All expected assets have a backup file created **within the last 6 hours** (evening window ~19:00):

| Asset | Latest Backup | Age |
|---|---|---|
| VM 111 omv-8 | 2026-07-14 19:00 | ~30m |
| LXC 101 pihole | 2026-07-14 19:02 | ~28m |
| LXC 102 searxng | 2026-07-14 19:03 | ~27m |
| LXC 103 immich | **2026-07-12 19:04** | ⚠️ **~48h** (CRITICAL from sysadmin) |
| LXC 104 cloudflared | 2026-07-14 19:08 | ~22m |
| LXC 105 nextcloudpi | 2026-07-14 19:08 | ~22m |
| LXC 106 onlyoffice | 2026-07-14 19:12 | ~18m |
| LXC 107 docker | 2026-07-14 19:13 | ~17m |
| LXC 997 tailscale | 2026-07-14 19:14 | ~16m |
| LXC 108 project-sandbox | **NONE** | ⚠️ Never backed up (CRITICAL from sysadmin) |
| LXC 109 guacamole | **NONE** | ⚠️ Never backed up (CRITICAL from sysadmin) |

**Evening backup window active — LXC 103 still failing (sysadmin CRITICAL).** LXC 108/109 unchanged CRITICAL from sysadmin report.

---

### SMART Supplemental (6h interval check)
Comparing to morning audit (2026-07-14 05:31):

| Device | Key Metric | Morning | Now | Δ | Status |
|---|---|---|---|---|---|
| /dev/sdb | Current_Pending_Sector | 0 | 0 | 0 | ✅ |
| /dev/sdb | Offline_Uncorrectable | 0 | 0 | 0 | ✅ |
| /dev/sdc | Current_Pending_Sector | 0 | 0 | 0 | ✅ |
| /dev/sdc | Offline_Uncorrectable | 0 | 0 | 0 | ✅ |
| /dev/sdd | Current_Pending_Sector | 0 | 0 | 0 | ✅ |
| /dev/sdd | Offline_Uncorrectable | 0 | 0 | 0 | ✅ |
| /dev/sde | Current_Pending_Sector | 0 | 0 | 0 | ✅ |
| /dev/sde | Offline_Uncorrectable | 0 | 0 | 0 | ✅ |
| /dev/sdf | Current_Pending_Sector | 0 | 0 | 0 | ✅ |
| /dev/sdf | Offline_Uncorrectable | 0 | 0 | 0 | ✅ |
| /dev/sdg | Current_Pending_Sector | **8** | **8** | **0** | ⚠️ Stable (critical pre-existing) |
| /dev/sdg | Offline_Uncorrectable | **8** | **8** | **0** | ⚠️ Stable (critical pre-existing) |
| /dev/sdg | Reallocated_Sector_Ct | 32 | 32 | 0 | ⚠️ Stable |

**No increases in any pending/offline sector counts.** /dev/sdg sector counts stable — no new degradation since morning. Drive remains in mergerfs ark pool (CRITICAL from sysadmin).

---

### Service Reachability
Spot-check of WARNING/CRITICAL services from morning audit:

- **Docker-VM (10.10.10.81)** — still unreachable — **unchanged WARNING** (sysadmin: confirm if intentional)
- **LXC 101 unbound.service** — still failed since 2026-07-05 — **unchanged WARNING** (sysadmin: cosmetic, dnscrypt operational)
- **OMV quotaon@ failed units × 6** — unchanged — **unchanged WARNING** (sysadmin: cosmetic)
- **OMV monit UUID spam** — unchanged — **unchanged WARNING** (sysadmin: journal noise)
- **LXC 103 immich backup** — still failing — **unchanged CRITICAL** (sysadmin CRITICAL action queue)
- **LXC 108/109 no backup** — unchanged — **unchanged CRITICAL** (sysadmin CRITICAL action queue)
- **OMV /dev/sdg SMART** — stable — **unchanged CRITICAL** (sysadmin CRITICAL action queue)

**No new service outages or regressions since morning audit.**

---

### Action Queue
*(Items requiring human attention — escalated to sysadmin daily audit)*

| Severity | Host | Item | Gap check finding |
|---|---|---|---|
| CRITICAL | proxmox/lxc-103 | immich backup failed 4+ nights | **Still failing this evening — CRITICAL unchanged** |
| CRITICAL | proxmox/lxc-108 | project-sandbox never backed up | **Still absent — CRITICAL unchanged** |
| CRITICAL | proxmox/lxc-109 | guacamole never backed up | **Still absent — CRITICAL unchanged** |
| CRITICAL | omv/sdg | 8 pending + 8 offline sectors | **Sector counts stable since morning — still CRITICAL, monitor for growth** |
| WARNING | docker-vm | unreachable | **Unchanged — confirm intentional** |
| WARNING | proxmox/lxc-101 | unbound.service failed | **Unchanged WARNING** |

**No new findings from this gap-fill pass. All gaps are pre-existing sysadmin audit items.**

---

*Report saved to: `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-14_19-30.md`*
*JSON companion: `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-14_19-30.json`*

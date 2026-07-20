# PLATFORM-RELIABILITY ROLLING HEALTH CHECK
## Gap vs sysadmin: Docker drift / LXC 997/104 / Backup jobs / SMART supplemental

**Generated:** 2026-07-15 07:03 WIB
**Gap focus:** mid-day drift since morning sysadmin audit (05:30)

---

### Status: 🟢 CLEAN — No drift detected

---

### Docker Drift
**Baseline:** Portainer + Dockge expected on LXC 107 (docker-vm 10.10.10.81 is decommissioned — workloads on LXC 107)

- `portainer` — **Up 9 days** — portainer/portainer-ce:latest ✅
- `dockge` — not present ✅ (compose stacks not deployed)
- `docker compose ls` — empty `[]` ✅
- **Docker drift: NONE.** Container state matches baseline.

---

### LXC Services (997 Tailscale, 104 Cloudflared)
These are NOT checked by sysadmin's daily audit — verified now:

- **LXC 997 Tailscale** — `tailscaled`: **active** ✅
- **LXC 104 Cloudflared** — `cloudflared`: **active** ✅
- **Both RUNNING.** No action needed.

---

### Backup Status
Morning backup window ran at ~02:00–02:15. Checking assets for any backup file created in last 6 hours:

| Asset | Latest Backup | Age | Status |
|---|---|---|---|
| VM 111 omv-8 | 2026-07-15 02:00 | ~5h | ✅ |
| LXC 101 pihole | **NONE since May 29** | — | ⚠️ STALE (pre-existing) |
| LXC 102 searxng | **NONE since Apr 14** | — | ⚠️ STALE (pre-existing) |
| LXC 103 immich | **NONE since Jul 12** | ~72h | 🔴 CRITICAL (pre-existing, 4 consecutive failures) |
| LXC 104 cloudflared | **NONE** | — | ⚠️ STALE (pre-existing) |
| LXC 105 nextcloudpi | **NONE** | — | ⚠️ STALE (pre-existing) |
| LXC 106 onlyoffice | 2026-07-15 02:11 | ~5h | ✅ |
| LXC 107 docker | 2026-07-15 02:12 | ~5h | ✅ |
| LXC 997 tailscale | 2026-07-15 02:13 | ~5h | ✅ |
| LXC 108 project-sandbox | **NONE** | — | 🔴 CRITICAL (pre-existing) |
| LXC 109 guacamole | **NONE** | — | 🔴 CRITICAL (pre-existing) |

> **Note:** The evening backup window (19:00–19:15) has not yet run at time of this check (~07:03). Morning backup window produced VM 111, LXC 106, 107, 997. All CRITICAL items are pre-existing and unchanged from sysadmin audit.

---

### SMART Supplemental (6h interval check)
Comparing to morning audit (2026-07-15 05:30):

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
Spot-check of WARNING/CRITICAL services from morning sysadmin audit:

- **Docker-VM (10.10.10.81)** — still unreachable — **unchanged WARNING** (decommissioned)
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
| CRITICAL | proxmox/lxc-103 | immich backup failed 4+ nights | **Still failing — CRITICAL unchanged** |
| CRITICAL | proxmox/lxc-108 | project-sandbox never backed up | **Still absent — CRITICAL unchanged** |
| CRITICAL | proxmox/lxc-109 | guacamole never backed up | **Still absent — CRITICAL unchanged** |
| CRITICAL | omv/sdg | 8 pending + 8 offline sectors | **Sector counts stable since morning — still CRITICAL, monitor for growth** |
| WARNING | docker-vm | unreachable | **Unchanged — decommissioned** |
| WARNING | proxmox/lxc-101 | unbound.service failed | **Unchanged WARNING** |

**No new findings from this gap-fill pass. All gaps are pre-existing sysadmin audit items.**

---

*Report saved to: `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-15_07-03.md`*
*JSON companion: `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-15_07-03.json`*

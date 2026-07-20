# PLATFORM-RELIABILITY ROLLING HEALTH CHECK
## Gap vs sysadmin: Docker drift / LXC 997/104 / Backup jobs / SMART supplemental
**Generated:** 2026-07-17 07:00 WIB
**Gap vs sysadmin deep audit (2026-07-17 05:30):** +1.5 hours

---

### Status: 🟡

**New drift since morning deep audit:**
- LXC 103 immich-server: still inactive — 7th consecutive backup failure (1 additional since morning)
- All other gap-fill checks passed

---

### Docker Drift
| Container | Status | Image |
|-----------|--------|-------|
| portainer | **Up 11 days** | portainer/portainer-ce:latest |

Docker-VM (10.10.10.81): UNREACHABLE — unchanged, deprecated TIER 3 host. All containers confirmed running inside LXC 107. No unexpected containers detected.

**Note:** Only `portainer` currently running. Baseline originally included `dockge` and `9router` — these appear absent from current `docker ps`. This is a **possible configuration change**, not a failure, but worth sysadmin verifying expected baseline.

---

### LXC Services (997 Tailscale, 104 Cloudflared)
| LXC | Service | Status |
|-----|---------|--------|
| 997 | tailscaled | **ACTIVE** |
| 104 | cloudflared | **ACTIVE** |

Both confirmed RUNNING. No issues.

---

### Backup Status
Nightly backup (Jul 17 02:00–02:14) completed with **9 success, 1 failed**:

| Asset | Archive | Age | Status |
|-------|---------|-----|--------|
| VM 111 omv-8 | vzdump-qemu-111-2026_07_17-02_00_03.vma.zst (10.9GB) | 5h | ✅ OK |
| LXC 997 tailscale | vzdump-lxc-997-2026_07_17-02_14_22.tar.zst (441MB) | 5h | ✅ OK |
| LXC 107 docker | backup successful | 5h | ✅ OK |
| LXC 106 onlyoffice | backup successful | 5h | ✅ OK |
| LXC 105 nextcloudpi | backup successful | 5h | ✅ OK |
| LXC 104 cloudflared | backup successful | 5h | ✅ OK |
| LXC 102 searxng | backup successful | 5h | ✅ OK |
| LXC 101 pihole | backup successful | 5h | ✅ OK |
| **LXC 103 immich** | **FAILED** | — | ❌ 7th consecutive |

Evening backup (Jul 16 19:04–19:16) also ran with **9 success, 1 failed** (LXC 103 TIMEOUT after 308s).

**LXC 103 immich — 7 consecutive failures** (nightly Jul 11/12/13/14/15/16/17 + evening Jul 16). No archive exists. immich-server is `inactive`. This is already CRITICAL in morning audit and has worsened by 1 additional failure cycle.

---

### SMART Supplemental
No OMV SMART checks completed within this interval (OMV ssh timeouts/blocking). Last known state from morning audit:
- `/dev/sdg` (ST2000LM007): Current_Pending=8, Offline_Uncorrectable=8 — unchanged from Jul 16 rolling check
- All other OMV disks: SMART overall-health PASSED

---

### Service Reachability
| Service | LXC | Morning Audit State | Observed (07:00) |
|---------|-----|---------------------|------------------|
| immich-server | 103 | CRITICAL (backup fail) | **inactive** (unchanged, worsening) |
| Docker-VM | — | UNREACHABLE | **UNREACHABLE** (unchanged) |
| pihole-FTL | 101 | WARNING (failed unit) | **not checked** (ssh blocked) |

---

### Action Queue
No new actions from this gap-fill. All items already tracked in morning deep audit:

1. **[CRITICAL] LXC 103 immich** — 7 consecutive backup failures; add `bwlimit=50000` to backup stanza; immich-server inactive
2. **[CRITICAL] OMV /dev/sdg** — 8 pending + 8 offline sectors; plan data evacuation and replacement
3. **[WARNING] Docker-VM** — confirm intentional shutdown, update inventory
4. **[WARNING] LXC 108/109** — not in backup CTS array
5. **[INFO] Only portainer running** — verify if dockge/9router removal from LXC 107 docker is expected

**Report saved:** `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-17_07-00.md`
**JSON saved:** `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-17_07-00.json`

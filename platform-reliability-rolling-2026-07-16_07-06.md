# PLATFORM-RELIABILITY ROLLING HEALTH CHECK
## Gap vs sysadmin: Docker drift / LXC 997/104 / Backup jobs / SMART supplemental
**Generated:** 2026-07-16 07:06 WIB
**Gap vs sysadmin deep audit (2026-07-16 05:30):** +1.6 hours

---

### Status: 🟡

**Drift since morning deep audit:**
- Docker-VM (10.10.10.81): still UNREACHABLE — not a new drift, already flagged in morning audit
- LXC 997 Tailscale: now confirmed ACTIVE (morning audit noted unknown due to host state)
- LXC 104 Cloudflared: now confirmed ACTIVE (morning audit noted unknown due to host state)

---

### Docker Drift
| Host | Status | ICMP | SSH |
|------|--------|------|-----|
| Docker-VM (10.10.10.81) | **UNREACHABLE** | 100% loss | timeout |

No container-level drift check possible — host is down (same state as morning audit). Workloads on LXC 107 unaffected.

---

### LXC Services (997 Tailscale, 104 Cloudflared)
| LXC | Service | Status |
|-----|---------|--------|
| 997 | tailscaled | **ACTIVE** |
| 104 | cloudflared | **ACTIVE** |

Both previously flagged as "unknown" in morning audit due to Docker-VM host being down. Both confirmed running at 07:06 WIB.

---

### Backup Status
AllLXCs with automated coverage produced fresh archives at ~02:00–02:16 today (5h ago):

| Asset | Latest Archive | Age |
|-------|----------------|-----|
| LXC 101 pihole | 2026-07-16 02:03 | 5h |
| LXC 102 searxng | 2026-07-16 02:04 | 5h |
| LXC 103 immich | **FAILED** (06th consecutive night) | — |
| LXC 104 cloudflared | 2026-07-16 02:09 | 5h |
| LXC 105 nextcloudpi | 2026-07-16 02:14 | 5h |
| LXC 106 onlyoffice | 2026-07-16 02:15 | 5h |
| LXC 107 docker | 2026-07-16 02:16 | 5h |
| LXC 997 tailscale | 2026-07-16 02:16 | 5h |
| VM 111 omv-8 | 2026-07-16 02:00 | 5h |

**LXC 103 (immich) new failure today** (2026-07-16 02:09 log): 6th consecutive nightly backup failure. No archive produced. **CRITICAL — unchanged since morning audit.**

---

### SMART Supplemental
| Disk | Pending Sectors | Offline Uncorrectable | Change vs Morning |
|------|----------------|-----------------------|-------------------|
| sdg (ST2000LM007) | 8 | 8 | **No increase** |

sdg values unchanged from morning audit (8/8). Disk is stable at elevated risk but no acute worsening detected in this interval.

---

### Service Reachability Spot-Check
From morning audit WARNING/CRITICAL items:

| Service | LXC | Expected State | Observed (07:06) |
|---------|-----|--------------|-----------------|
| immich-server | 103 | CRITICAL (backup fail) | **inactive** |
| unbound | 101 | WARNING (failed unit) | **failed** (unchanged) |
| pihole-FTL | 101 | running | **active** |
| Docker-VM | — | UNREACHABLE | **UNREACHABLE** (unchanged) |

---

### Action Queue
No new actions from this gap-fill run. All items are already queued in the morning deep audit:

1. **[CRITICAL] LXC 103 immich backup** — 6 consecutive failures; add `bwlimit=50000` to backup stanza
2. **[CRITICAL] OMV /dev/sdg** — 8 pending + 8 offline sectors; plan data evacuation and replacement
3. **[WARNING] Docker-VM** — confirm intentional shutdown and update inventory
4. **[WARNING] LXC 108/109** — not in backup CTS array (already known)

**Report saved:** `/Users/beem/homelab-reports/platform-reliability-rolling-2026-07-16_07-06.md`

# PLATFORM-RELIABILITY ROLLING HEALTH CHECK
## Gap vs sysadmin: Docker drift / LXC 997/104 / Backup jobs / SMART supplemental

### Status: 🔴

Current read-only state at 2026-07-18 07:03 WIB shows a critical ongoing gap: LXC 103 Immich remains inactive and its backup timed out again. Tailscale and Cloudflared are healthy; the latest backup cycle produced artifacts within six hours for 8 guests but ended `Success=9 Failed=1`.

### Docker Drift
- Checked Docker in LXC 107 (current topology; deprecated Docker-VM `.81` was not used).
- Running container: `portainer` — Up 12 days.
- No unexpected container names observed.
- Baseline gaps: expected `dockge` and `9router` were not present.
- `docker compose ls --all --format json` returned `[]`.

### LXC Services (997 Tailscale, 104 Cloudflared)
- LXC 997: `tailscaled` **active**; container **RUNNING**.
- LXC 104: `cloudflared` **active**; container **RUNNING**.

### Backup Status
- Latest successful artifacts were created 2026-07-18 02:02–02:15 WIB, approximately 4h48m before this check; within the six-hour window.
- Successful examples: VM 111 (11G), CT 105 (7.2G), CT 107 (414M), CT 997 (441M).
- Backup cycle summary: `Success=9 Failed=1`.
- CT 103 (Immich) timed out after 300 seconds at 02:09:33; its backup remains missing/failed.
- `pvesm list backup-nas` did not complete because the backup-NFS query hung; evidence above comes from the live Proxmox backup log and mounted NFS state.

### SMART Supplemental
- `/dev/sdg`: Current_Pending_Sector **8**, Offline_Uncorrectable **8**, Reallocated **32** — unchanged versus the 2026-07-09 sysadmin baseline; no increase detected.
- `/dev/sdd`: SMART overall PASSED, but airflow temperature attribute is `FAILING_NOW` at **55°C**; pending and offline-uncorrectable remain 0.
- Other checked disks did not show a new pending/uncorrectable-sector increase.

### Service Reachability
- LXC 103 `immich-server`: **inactive** — still down.
- LXC 101 `unbound`: **failed** — still down; prior fallback DNS path remains outside this spot-check.
- Proxmox reachable; VM 111 running.
- OMV reachable; SMART probes completed.

### Action Queue
- [ ] **Critical:** investigate/repair LXC 103 Immich service and backup timeout; backup coverage remains absent.
- [ ] **Warning:** reconcile Docker baseline: determine whether `dockge` and `9router` were intentionally removed or are missing mid-day.
- [ ] **Warning:** inspect OMV cooling/temperature for `/dev/sdd` (55°C, `FAILING_NOW`) during the next sysadmin deep audit.
- [ ] **Warning:** repair LXC 101 `unbound.service` failure.

### Gap vs sysadmin Audit
- This rolling check is a gap-fill, not a replacement for the daily deep audit. It adds current Docker drift, LXC 997/104 tunnel-service state, six-hour backup-window evidence, and supplemental SMART readings.
- It confirms the sysadmin-known Immich/backup problem is still active and adds the missing Docker baseline services plus the current `/dev/sdd` temperature warning.

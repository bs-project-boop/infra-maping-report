# HOMELAB DEEP AUDIT — BLOCKED
Generated: 2026-07-13 05:35:38 WIB

Blocked hosts: ['docker-vm']

Reachability evidence:
```json
{
  "proxmox": {
    "ip": "10.10.10.2",
    "ping": {
      "rc": 0,
      "out": "PING 10.10.10.2 (10.10.10.2): 56 data bytes\n64 bytes from 10.10.10.2: icmp_seq=0 ttl=64 time=2.554 ms\n64 bytes from 10.10.10.2: icmp_seq=1 ttl=64 time=3.182 ms\n\n--- 10.10.10.2 ping statistics ---\n2 packets transmitted, 2 packets received, 0.0% packet loss\nround-trip min/avg/max/stddev = 2.554/2.868/3.182/0.314 ms\n",
      "err": ""
    },
    "ssh": {
      "rc": 0,
      "out": "pve\nMon Jul 13 05:35:40 AM WIB 2026\n",
      "err": ""
    }
  },
  "docker-vm": {
    "ip": "10.10.10.81",
    "ping": {
      "rc": 2,
      "out": "PING 10.10.10.81 (10.10.10.81): 56 data bytes\nRequest timeout for icmp_seq 0\n\n--- 10.10.10.81 ping statistics ---\n2 packets transmitted, 0 packets received, 100.0% packet loss\n",
      "err": ""
    },
    "ssh": {
      "rc": 255,
      "out": "",
      "err": "ssh: connect to host 10.10.10.81 port 22: Operation timed out\n"
    }
  },
  "omv": {
    "ip": "10.10.10.82",
    "ping": {
      "rc": 0,
      "out": "PING 10.10.10.82 (10.10.10.82): 56 data bytes\n64 bytes from 10.10.10.82: icmp_seq=1 ttl=64 time=2.749 ms\n\n--- 10.10.10.82 ping statistics ---\n2 packets transmitted, 2 packets received, 0.0% packet loss, 1 packets out of wait time\nround-trip min/avg/max/stddev = 2.749/4.329/5.908/1.580 ms\n",
      "err": ""
    },
    "ssh": {
      "rc": 0,
      "out": "openmediavault8\nMon Jul 13 05:35:58 WIB 2026\n",
      "err": ""
    }
  }
}
```

Impact: deep audit sections for blocked hosts are unavailable; main report remains CRITICAL.

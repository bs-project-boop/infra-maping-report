# HOMELAB DEEP AUDIT — BLOCKED
Generated: 2026-07-13 05:34:19 WIB

Blocked hosts: ['docker-vm']

Reachability evidence:
```json
{
  "proxmox": {
    "ip": "10.10.10.2",
    "ping": {
      "rc": 0,
      "out": "PING 10.10.10.2 (10.10.10.2): 56 data bytes\n64 bytes from 10.10.10.2: icmp_seq=0 ttl=64 time=2.508 ms\n64 bytes from 10.10.10.2: icmp_seq=1 ttl=64 time=3.550 ms\n\n--- 10.10.10.2 ping statistics ---\n2 packets transmitted, 2 packets received, 0.0% packet loss\nround-trip min/avg/max/stddev = 2.508/3.029/3.550/0.521 ms\n",
      "err": ""
    },
    "ssh": {
      "rc": 0,
      "out": "pve\nMon Jul 13 05:34:21 AM WIB 2026\n",
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
      "out": "PING 10.10.10.82 (10.10.10.82): 56 data bytes\n64 bytes from 10.10.10.82: icmp_seq=0 ttl=64 time=3.510 ms\n\n--- 10.10.10.82 ping statistics ---\n2 packets transmitted, 2 packets received, 0.0% packet loss, 1 packets out of wait time\nround-trip min/avg/max/stddev = 3.510/4.290/5.070/0.780 ms\n",
      "err": ""
    },
    "ssh": {
      "rc": 0,
      "out": "openmediavault8\nMon Jul 13 05:34:39 WIB 2026\n",
      "err": ""
    }
  }
}
```

Impact: deep audit sections for blocked hosts are unavailable; main report remains CRITICAL.

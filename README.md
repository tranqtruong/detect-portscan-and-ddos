# 🛡️ Snort-based Intrusion Detection & DDoS Prevention

🎥 **Demo video**: [Watch on YouTube](https://youtu.be/_GiEPn2n-hU)

A personal project to deploy and configure **Snort** in IPS mode for detecting **port scanning** and **mitigating DoS/DDoS attacks** on a simulated network.

> 🔍 Final report for the course: *Network Security Monitoring Techniques* – PTIT  
> 👤 Developed individually by: **Trần Quốc Trượng** (despite group title)

---

## 🔧 Lab Architecture

- **Snort IDS/IPS**: Ubuntu VM, dual NIC (inline mode)
- **Attacker machines**: Windows 8.1 (LOIC, custom scripts)
- **Target**: Apache Web Server on Ubuntu 20.04

Traffic flow:  
Attacker → Snort (NFQUEUE 2) → Target

---

## 🚨 Detection & Mitigation Capabilities

✅ **Port Scanning Detection**
- TCP SYN scan, UDP scan via `nmap`
- Custom Snort rules triggering alerts on scan signatures

✅ **DDoS/DoS Attack Blocking**
- Ping of Death (ICMP flood)
- TCP SYN Flood (LOIC tool)
- HTTP GET Flood (HTTPdos tool)
- Uses `NFQUEUE` with `iptables` to drop packets in-line

---

## 📁 Key Components

| File                          | Description                         |
|-------------------------------|-------------------------------------|
| `snort.conf`                  | Main Snort config file              |
| `local.rules`                 | Custom rules for DoS/DDoS detection |
| `iptables` config             | NFQUEUE redirection setup           |
| `demo-screenshots/`           | Screenshots of live detection       |

---

## 🧪 Sample Rule Example

```snort
drop tcp any any -> 10.10.10.128 any (flags: S; msg:"SYN Flood attempt"; threshold: type both, track by_dst, count 1000, seconds 3; sid:10001; rev:1;)

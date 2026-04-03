# Homelab — April 2026

> Snapshot of my self-hosted services running as of April 2026.

![Dashboard](dashboard-april-2026.png)

---

## ✨ Infrastructure

### <span style="color:#2ea043">➜</span> Proxmox VE (N150Cluster)
High-availability Proxmox VE cluster running on two **GMKTEC NUCBOX G3 Plus** nodes, each equipped with an **Intel N150** processor, **32GB RAM**, and a **1TB NVMe SSD**. Zero-downtime resilience is achieved via ZFS-replication, syncing mission-critical VMs to the secondary node at 15-minute intervals.

A third standalone node — **x03.lan** — handles less critical workloads and serves as the cluster's **Corosync quorum Qdevice** (Intel NUC 11, N6005, 32GB RAM, 1TB NVMe SSD).

| Node | Role | Hardware | Status |
|------|------|----------|--------|
| x01.lan | Cluster member | N150 / 32GB / 1TB NVMe | Active |
| x02.lan | Cluster member | N150 / 32GB / 1TB NVMe | Active |
| x03.lan | Standalone + Qdevice | N6005 / 32GB / 1TB NVMe | Active |

**Cluster:** 2-node knet transport, secure authentication enabled
**Quorum:** Quorate with Qdevice — expected votes 3, quorum 2

### <span style="color:#2ea043">➜</span> Windows Server 2022
On-premises identity and directory infrastructure running **Active Directory Domain Services**, providing user and computer authentication across the homelab. Also serves as the **DHCP server** for the LAN and runs the **MS DNS role** for internal name resolution.

### <span style="color:#2ea043">➜</span> Proxmox Backup Server (PBS)
Dedicated backup solution for the Proxmox cluster, handling incremental backups and snapshot management for all VMs and containers.

### <span style="color:#2ea043">➜</span> Homepage
Modern, fully static, fast, and highly customizable application dashboard. Configured via YAML files with widget integrations for all major services. CPU and temperature monitoring powered by **Glances**.

### <span style="color:#2ea043">➜</span> Pulse
Single pane of glass monitoring for the entire Proxmox infrastructure — provides unified dashboard, Telegram alerts, temperature monitoring, SSD health/disk life, backup overview, and AI-powered insights. Replaces the need for multiple monitoring tools with one cohesive interface.

![Pulse Dashboard](pulse-screenshot.png)

---

## 🚀 Network

### <span style="color:#2ea043">➜</span> Pi-hole (×2)
Two instances of network-wide ad blocker running on the LAN. Blocks ads and trackers at the DNS level with live query statistics and gravity blocklist management.

### <span style="color:#2ea043">➜</span> UniFi OS Server
Management platform for Ubiquiti networking gear.

### <span style="color:#2ea043">➜</span> MySpeed
Self-hosted internet speed test tracker that logs and displays historical download/upload speeds and ping metrics.

### <span style="color:#2ea043">➜</span> Uptime Kuma
Self-hosted monitoring tool tracking the uptime and response time of internal and external services — currently showing 8 sites up at 100% uptime.

---

## 💾 Storage

### <span style="color:#2ea043">➜</span> Synology DS423+ (NAS)
DiskStation NAS running **DSM 7** on an ARM-based processor with a **4× 2.5" SSD configuration**:

| Storage Pool | Drives | Configuration | Use Case |
|-------------|--------|--------------|----------|
| Storage Pool 1 | 2× 1TB WD Red SA500 | RAID 1 (BTRFS) | Primary data store | 576.8 GiB available |
| Storage Pool 2 | 2× 500GB Samsung 860 EVO | RAID 0 (BTRFS) | Media, backups, ISO libraries, Time Machine target | 303.5 GiB available |

Both pools use **BTRFS** with periodic snapshots for data protection. A custom **Realtek NIC driver from bb-qq** provides a **2.5Gbit USB adapter** based on the **RTL8156B** chip, delivering reliable high-speed connectivity.

- **Services:** Synology Photos, Notes Station, Download Station
- **Uptime:** Months of continuous operation with zero packet drops recorded

---

## 🎬 Media

### <span style="color:#2ea043">➜</span> Plex
Media server streaming personal video and music libraries to devices across the network. Currently serving 1 active stream.

- **Library:** 87 movies, 6 TV shows, 130 albums

---

## 🏠 Home Automation

### <span style="color:#2ea043">➜</span> Home Assistant
Central hub for smart home integration. Manages lights, switches, and presence detection.

- **Status:** 1/1 people home, 4/4 lights on, 0/1 switches on

---

*Last updated: April 2026*

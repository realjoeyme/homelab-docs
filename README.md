# Homelab — April 2026

> Snapshot of my self-hosted services running as of April 2026.

![Dashboard](dashboard-april-2026.png)

---

## ✨ Infrastructure

### Proxmox VE (N150Cluster)
High-availability Proxmox VE cluster running on two **GMKTEC NUCBOX G3 Plus** nodes, each equipped with an **Intel N150** processor, **32GB RAM**, and a **1TB NVMe SSD**. Zero-downtime resilience is achieved via ZFS-replication, syncing mission-critical VMs to the secondary node at 15-minute intervals.

A third standalone node — **x03.lan** — handles less critical workloads and serves as the cluster's **Corosync quorum Qdevice** (Intel NUC 11, N6005, 32GB RAM, 1TB NVMe SSD).

| Node | Role | Hardware | Status |
|------|------|----------|--------|
| x01.lan | Cluster member | N150 / 32GB / 1TB NVMe | Active |
| x02.lan | Cluster member | N150 / 32GB / 1TB NVMe | Active |
| x03.lan | Standalone + Qdevice | N6005 / 32GB / 1TB NVMe | Active |

**Cluster:** 2-node knet transport, secure authentication enabled
**Quorum:** Quorate with Qdevice — expected votes 3, quorum 2

### Proxmox Backup Server (PBS)
Dedicated backup solution for the Proxmox cluster, handling incremental backups and snapshot management for all VMs and containers.

### Pulse
Single pane of glass monitoring for the entire Proxmox infrastructure — provides unified dashboard, Telegram alerts, temperature monitoring, SSD health/disk life, backup overview, and AI-powered insights. Replaces the need for multiple monitoring tools with one cohesive interface.

![Pulse Dashboard](pulse-screenshot.png)

---

## 🚀 Network

### Pi-hole (×2)
Two instances of network-wide ad blocker running on the LAN. Blocks ads and trackers at the DNS level with live query statistics and gravity blocklist management.

### UniFi OS Server
Management platform for Ubiquiti networking gear — switches, access points, and security gateways.

### MySpeed
Self-hosted internet speed test tracker that logs and displays historical download/upload speeds and ping metrics.

### Uptime Kuma
Self-hosted monitoring tool tracking the uptime and response time of internal and external services — currently showing 8 sites up at 100% uptime.

---

## 💾 Storage

### Synology DS423+ (NAS)
Network-attached storage serving as the central file store for the homelab. Hosts media libraries, photos, and download station.

- **Storage:** 303.5 GiB available
- **Services:** Synology Photos, Notes Station, Download Station

---

## 🎬 Media

### Plex
Media server streaming personal video and music libraries to devices across the network. Currently serving 1 active stream.

- **Library:** 87 movies, 6 TV shows, 130 albums

---

## 🏠 Home Automation

### Home Assistant
Central hub for smart home integration. Manages lights, switches, and presence detection.

- **Status:** 1/1 people home, 4/4 lights on, 0/1 switches on

---

*Last updated: April 2026*

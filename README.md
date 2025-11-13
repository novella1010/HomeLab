# 🏠 Homelab Infrastructure

A fully segmented, security-focused homelab designed to emulate enterprise networking, virtualization, monitoring, and Zero-Trust principles. This lab includes pfSense firewalling, VLAN routing, Dockerized services, centralized logging, and multi-node monitoring across Debian, Proxmox, and Raspberry Pi hosts.

---

## 📸 Network Diagram
*(See `HomeLab_Network_map_2.png` in repo)*  
Diagram includes pfSense, Netgear switch, TP-Link AP, Debian Docker host, Proxmox, and Raspberry Pi nodes.

---

## 🔐 pfSense Firewall – `10.0.10.1`

The core routing, segmentation, and security enforcement point.

### Key Features
- VLAN segmentation (10, 20, 30, 40, 50, 99, 999)
- **CrowdSec LAPI** for automated IP banning
- **Reverse Proxy SSL Offloading** via NGINX Proxy Manager
- Port forwarding: `443 → NPM`
- Cloudflare DDNS integration
- IDS/IPS capabilities (Suricata/Snort)
- pfBlockerNG with IP list feed downloaded with active blocking.
- NAT + policy-based routing + firewall rule isolation

---

## 🖧 Netgear Managed Switch – `10.0.10.2`

Enterprise L2 switching and VLAN distribution.

### Configuration
- 802.1Q VLANs: `10, 20, 30, 40, 50, 99, 999`
- Port 1 configured as **trunk uplink to pfSense**
- All other ports mapped to containers, VMs, AP, and Pi nodes
- IGMP Snooping Enabled on Trunk Port 1
- CrowdSec log parser enabled for switch syslogs

---

## 📡 TP-Link Access Point – `10.0.10.70`

Multi-SSID access point mapped to VLANs for wireless segmentation.

### SSIDs / VLANs
- **VLAN 20** – Personal devices (`Todo de Ti`)
- **VLAN 30** – Guest (`Guest`)
- **VLAN 40** – IoT  
  - IoT 5GHz  
  - IoT 2.4GHz (LORAZAPEM)

Ensures isolation between trusted devices, guests, and IoT.

---

## 🧰 Debian Docker Host – `10.0.10.22`

Main container host running security, monitoring, and proxy services.

### Dockerized Services
- **Nginx Proxy Manager**
- **CrowdSec Reverse Proxy Parser**
- **Zabbix Agent Monitoring**
- **NetData Monitoring**
- **Dozzle Log Viewer**
- CrowdSec agent for distributed log ingestion

Serves as the hub for reverse proxy entry and monitoring connectivity.

---

## 🖥️ Proxmox Virtualization – `10.0.10.40`

Hypervisor hosting production and testing VMs.

### Virtual Machines
- **Mailserver VM** (Mailrise, Apprise) – `10.0.10.105`
- **Zabbix Server VM** – `10.0.10.102`
- **Ansible VM** - `10.0.10.101`
- **Kali Linux VM** - `10.96.12.10`
- **CrowdSec Proxmox Log Parser**
- VLAN-aware bridges for isolated VM networks

Used for labs, service hosting, and orchestration.

---

## 🍓 Raspberry Pi Node – `10.0.50.10`

Automation and lightweight monitoring node running on its own VLAN.

### Services
- **HomeAssistant**
- **CrowdSec Log Parser / Agent**
- **Dozzle (Remote Host)**
- **NetData Agent**
- **Node Exporter**

Communicates securely with the Debian host and Proxmox monitoring stack.

---

## 🔧 VLAN Segmentation Summary

| VLAN | Name / Purpose |
|------|----------------|
| **10** | Internal trusted devices & servers |
| **20** | Wireless personal devices |
| **30** | Guest WiFi |
| **40** | IoT devices |
|

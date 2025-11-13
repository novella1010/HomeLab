🏠 Homelab Infrastructure – Overview

This homelab environment is built to simulate an enterprise-grade network with VLAN segmentation, centralized logging, containerized services, reverse proxying, and security monitoring. It is designed for hands-on practice with firewalls, virtualization, monitoring stacks, and Zero-Trust principles.

🔐 Firewall & Edge
pfSense – 10.0.10.1

The core of the network, acting as the primary router and firewall.

Key Features:

VLAN segmentation for internal, AP, Guest, IoT, Lab, and special-purpose networks

CrowdSec LAPI for automated intrusion prevention

HAProxy / NGINX Proxy Manager SSL Offloading

Inbound 443 → NPM Proxy

Cloudflare DDNS integration

Suricata/Snort IDS/IPS (optional)

Full NAT + policy routing

🖧 Switching
Netgear Managed Switch – 10.0.10.2

Configured as the central layer-2 distribution switch.

Highlights:

802.1Q VLANs: 10, 20, 30, 40, 50, 99, 999

Port 1 is the trunk uplink to pfSense

Access ports mapped to Debian, VM hosts, AP, and Raspberry Pi

Jumbo frame support and log parsing agents for CrowdSec

📡 Wireless
TP-Link Access Point – 10.0.10.70

Broadcasts multiple segmented SSIDs mapped to VLANs:

VLAN 20 – Personal devices (e.g., phones, laptops)

VLAN 30 – Guest network

VLAN 40 – IoT (2.4 GHz + 5 GHz SSIDs)

Provides clean isolation for IoT devices, guests, and trusted devices.

🧰 Core Services & Virtual Machines
Debian Host – 10.0.10.22

Runs Docker and major monitoring components.

Containers include:

CrowdSec Reverse Proxy Parser

Zabbix Agent Monitoring

Netdata Monitoring

Dozzle Log Viewer

Nginx Proxy Manager

Background services for log ingestion and metrics forwarding

Proxmox Node – 10.0.10.40

Virtualization node hosting production and lab VMs.

Sample VMs:

mailserver VM (Mailrise, Apprise) – 10.0.10.105

Zabbix Server VM – 10.0.10.102

Kali VM

Ansible VM

Connected via VLAN-aware bridges for isolation and flexibility.

🍓 Raspberry Pi Node – 10.0.50.10

Dedicated lightweight node used for home automation and remote telemetry.

Services:

HomeAssistant

CrowdSec Log Parser / Agent

Dozzle (Remote Host)

NetData Service Agent

Runs on its own subnet (VLAN 50) but communicates securely with the main monitoring stack.

🔧 Network Segmentation Summary
VLAN	Purpose
10	Internal trusted devices + servers
20	Wireless personal devices
30	Guest WiFi
40	IoT (cameras, smart plugs, appliances)
50	Raspberry Pi & automation
99	Lab WAN for virtual pfSense testing
999	Management / reserved
🎯 Project Goals

Build a secure, segmented home network resembling enterprise environments

Practice firewalling, security automation, monitoring, and SIEM ingestion

Self-host critical services with SSL, monitoring, and strict access control

Test new deployments in isolated VLANs and virtualization environments

Gain hands-on experience with threat detection and network observability

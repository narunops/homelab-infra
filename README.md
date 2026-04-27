# homelab-infrastructure
Personal infrastructure lab running production-grade services on consumer hardware. Built to practice IT/OT security, network 
segmentation, system administration, and infrastructure monitoring

---

## Hardware

| Device | Specs | Role |
|--------|-------|------|
| HP EliteBook 840 G3 | 16GB RAM, 256GB NVMe, Arch Linux | Gateway · Firewall · Monitoring · DNS |
| HP ProDesk 600 G5 SFF | 8GB RAM, 256GB NVMe, Proxmox VE | Primary hypervisor — VMs and LXC |
| Nokia ADSL modem | 4-port | Dumb switch — DHCP disabled |
| Mobile hotspot | 4G / phone | WAN uplink (wlan0) |

## Overview

This homelab simulates a small infrastructure environment with:

- Linux gateway
- NAT and IPv4 forwarding
- Virtualization using Proxmox
- Infrastructure monitoring
- DNS filtering
- SSH-based remote management

The lab is used to practice **Linux administration, networking, and infrastructure monitoring**.

---
                              
## Network Architecture

![Network Diagram](network-diagram.png)

## Network zones
| Zone           | Subnet      | Purpose                        |
|-----          -|--------     |---------                       |
| Infrastructure | 192.168.x.x | Proxmox, VMs, LXC — static IPs |
| Guest | 10.x.x.x | Client devices — isolated from infra       |

## Services

| Service        | Platform | Purpose |

| OPNsense       | Arch Linux (bare metal) | Firewall, NAT, zone isolation |
| Zabbix server  | Arch Linux | Infrastructure monitoring |
| Pi-hole        | Arch Linux | DNS filtering, Cloudflare upstream |
| Proxmox VE     | HP ProDesk | Hypervisor |
| AlmaLinux VM   | Proxmox | RHEL-compatible test environment |
| Windows Server | Proxmox VM | Active Directory (in progress) |
| Snipe-IT       | Proxmox LXC | IT asset management |

## Skills demonstrated
- Network segmentation and firewall rule management (OPNsense)
- Infrastructure monitoring with Zabbix server/agent architecture
- DNS filtering and recursive resolver configuration
- Hypervisor management — VM and LXC lifecycle
- Linux system administration (Arch, AlmaLinux)
- NAT, IPv4 forwarding, MASQUERADE on Linux

## Future Improvements

- Deploy Snipe-IT container
- Add SIEM monitoring
- Infrastructure automation scripts
- Expand monitoring dashboards

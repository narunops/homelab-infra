
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
| Mobile hotspot | Samsung infra | WAN uplink (wlan0) |

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

[![Screenshot-2026-04-27-170457.jpg](https://i.postimg.cc/t4tcNPWv/Screenshot-2026-04-27-170457.jpg)](https://postimg.cc/v4TXQ4P5)


## Network zones

| Zone           | Subnet      | Purpose                        |
| Infrastructure | 192.168.x.x | Proxmox, VMs, LXC — static IPs |
| Guest | 10.x.x.x | Client devices — isolated from infra       |

**OPNsense** runs on the Arch laptop and enforces zone separation.  
Proxmox nodes and VMs are unreachable from the guest network by firewall rule.



## Services

| Service       | Host                    | Purpose |

| OPNsense      | Arch Linux (bare metal) | Firewall, NAT, dual-zone isolation |
| Zabbix server | Arch Linux | Infrastructure monitoring |
| Pi-hole       | Arch Linux | DNS filtering · Cloudflare 1.1.1.1 upstream |
| Proxmox VE    | HP ProDesk | Hypervisor — VM and LXC management |
| AlmaLinux VM  | Proxmox | RHEL-compatible test environment |
| Windows Server VM | Proxmox | Active Directory lab (in progress) |
| Snipe-IT      | Proxmox LXC | IT asset management |
| Zabbix agent  | Proxmox node | Sends metrics to Zabbix server on Arch |


## Virtual Machines & Containers

**Proxmox Node 1 — HP ProDesk 600 G5 SFF**  
Static IP: `192.168.1.101` · Headless · managed via SSH from Arch Konsole

| ID | Type | OS | Purpose |
|----|------|----|---------|
| 100 | VM | AlmaLinux | RHEL-compatible server testing |
| 101 | VM | Windows Server | Active Directory domain lab |
| 200 | LXC | Debian | Snipe-IT IT asset management |

LXC containers are created and destroyed as needed — ephemeral 
workloads for testing services without committing VM resources.

## Monitoring

Zabbix server runs on the Arch laptop and collects metrics from a Zabbix agent installed on the Proxmox node.

Monitored items:
- Proxmox host CPU, RAM, disk utilisation
- Network interface traffic
- System uptime and process health

Pi-hole provides network-wide DNS filtering with Cloudflare 1.1.1.1 as upstream resolver. 
DNS queries from all LAN clients route through Pi-hole regardless of zone.


## Network Segmentation Detail

| Zone           | Subnet         | DHCP               | Access |

| Infrastructure | 192.168.1.0/24 | Static assignments | Proxmox, VMs, LXC only |
| Guest          | 10.x.x.x/24    | OPNsense DHCP      | Client devices, office laptop |

Firewall rules block guest zone from reaching any infrastructure zone address. 
Zabbix and Pi-hole are accessible only from the infrastructure zone.


## Skills Demonstrated

| Skill                             | Evidence 
| Firewall and network segmentation | OPNsense dual-zone config with isolation rules 
| Linux system administration       | Arch Linux as production gateway, daily driver 
| Hypervisor management             | Proxmox VE — VM and LXC lifecycle management 
| Infrastructure monitoring         | Zabbix server + agent architecture across hosts 
| DNS management                    | Pi-hole with custom upstream, LAN-wide filtering 
| NAT and IP routing                | IPv4 forwarding, MASQUERADE, multi-zone routing 
| Remote administration             | Headless Proxmox node managed entirely via SSH 
| Asset management                  | Snipe-IT tracking homelab hardware and services


## Planned

- [ ] Wazuh SIEM — deploy on AlmaLinux VM, agent on all nodes
- [ ] Active Directory — promote Windows Server to domain controller
- [ ] Join AlmaLinux to AD domain via SSSD
- [ ] pfSense/OPNsense config backup — version-controlled exports
- [ ] Bash scripts for LXC provisioning automation
- [ ] Grafana dashboards on top of Zabbix data


## Certifications (in progress)

| Cert                    | Status |

| ISC2 CC — Certified in Cybersecurity | Exam scheduled 2026 |
| Schneider Electric DCCA | Completed |
| CISA ICS 100W / 210W-03 | Completed |
| Cisco Networking Basics | Completed |
| Cisco Introduction to Cybersecurity | Completed |


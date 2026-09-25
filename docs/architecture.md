# Homelab Architecture

## Overview

This homelab simulates a small enterprise IT environment designed around network segmentation, centralized identity, infrastructure monitoring, logging, documentation, and automation.

The environment is hosted on a Dell PowerEdge R720 running Proxmox VE and incorporates both physical and virtual infrastructure.

## Physical Infrastructure

| Device | Role | Management IP | Purpose |
|---|---|---|---|
| Dell PowerEdge R720 (PVE) | Hypervisor | 192.168.1.200/24 | Hosts the virtual infrastructure |
| Cisco SG200-50 (SW01) | Managed Switch | — | Physical switching and VLAN connectivity |

### Physical Connections

| Source | Destination | Purpose |
|---|---|---|
| PVE nic0 | SW01 ge5 | Proxmox management connectivity |
| PVE nic1 | SW01 ge6 | Lab network / VLAN connectivity |

## Network Segmentation

| VLAN | Name | Subnet | Purpose |
|---|---|---|---|
| 10 | CLIENTS | 10.10.10.0/24 | Domain-joined client systems |
| 20 | SERVERS | 10.10.20.0/24 | Servers and infrastructure services |

OPNsense provides routing, firewalling, DHCP, NAT, and inter-VLAN connectivity for the lab environment.

## Virtual Infrastructure

| System | IP Address | Role |
|---|---|---|
| OPNsense | 10.10.20.1/24 | Firewall, routing, NAT, DHCP |
| DC01 | 10.10.20.11/24 | Active Directory Domain Services and DNS |
| NMS01 | 10.10.20.12/24 | LibreNMS monitoring and centralized syslog |
| GRAFANA01 | 10.10.20.13/24 | Grafana and Prometheus monitoring |
| NETBOX01 | 10.10.20.14/24 | NetBox IPAM and infrastructure source of truth |
| ANSIBLE01 | 10.10.20.15/24 | Ansible automation and configuration management |
| WINCLIENT01 | 10.10.10.191/24 | Windows domain client |

## Active Directory

The Windows environment uses the following Active Directory domain:

`corp.example.test`

DC01 provides:

- Active Directory Domain Services
- DNS
- Domain authentication
- Organizational unit and group management
- Group Policy

WINCLIENT01 is joined to the domain and resides on the CLIENTS VLAN.

## Monitoring and Logging

The environment uses multiple monitoring technologies to provide visibility across different infrastructure layers.

### LibreNMS

LibreNMS provides SNMP-based infrastructure monitoring including:

- Device availability
- CPU utilization
- Memory utilization
- Network traffic
- Interface statistics
- Historical performance data

### Grafana and Prometheus

Prometheus collects metrics from Linux Node Exporter and Windows Exporter.

Grafana provides dashboards for:

- CPU utilization
- RAM utilization
- Disk utilization
- Network traffic
- System load
- System uptime

### Centralized Syslog

OPNsense firewall logs are forwarded to NMS01 and stored centrally for traffic and firewall-event analysis.

## Infrastructure Documentation

NetBox serves as the source of truth for the environment and documents:

- Sites
- VLANs
- IP prefixes
- IP addresses
- Physical devices
- Virtual machines
- Interfaces
- Physical cabling
- Proxmox virtualization resources

## Automation

ANSIBLE01 provides centralized configuration management for Linux infrastructure.

Ansible uses SSH key authentication to manage:

- NMS01
- GRAFANA01
- NETBOX01

Automation testing includes multi-host connectivity, fact gathering, infrastructure auditing, and remote system-state validation.

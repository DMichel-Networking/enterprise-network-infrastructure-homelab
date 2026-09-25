# Enterprise Network Infrastructure Homelab

A self-hosted enterprise IT infrastructure lab built to develop hands-on experience with networking, systems administration, monitoring, infrastructure documentation, and automation.

The environment runs on a Dell PowerEdge R720 using Proxmox VE and combines segmented networking, Active Directory, centralized monitoring and logging, IP address management, and Ansible automation.

## Project Objectives

The goal of this project was to build and operate a realistic small-enterprise environment rather than isolated virtual machines.

Key objectives included:

- Design a segmented network using VLANs and 802.1Q
- Deploy routing and firewall services with OPNsense
- Build a Windows Active Directory domain
- Implement centralized DNS and domain authentication
- Monitor infrastructure using SNMP, Prometheus, and Grafana
- Centralize firewall logs for troubleshooting and analysis
- Maintain an infrastructure source of truth with NetBox
- Automate Linux administration using Ansible
- Document physical and virtual infrastructure
- Validate connectivity and services through structured testing

## Architecture

### Physical Infrastructure

- Dell PowerEdge R720 running Proxmox VE
- Cisco SG200-50 managed switch
- OPNsense virtual firewall/router
- Dedicated Proxmox management and lab network interfaces

### Network Segmentation

| VLAN | Name | Network | Purpose |
|---|---|---|---|
| 10 | CLIENTS | `10.10.10.0/24` | Domain client systems |
| 20 | SERVERS | `10.10.20.0/24` | Infrastructure and server workloads |

### Infrastructure Services

| System | Address | Function |
|---|---|---|
| OPNsense | `10.10.20.1` | Firewall, routing, NAT, DHCP |
| DC01 | `10.10.20.11` | Active Directory and DNS |
| NMS01 | `10.10.20.12` | LibreNMS and centralized syslog |
| GRAFANA01 | `10.10.20.13` | Grafana and Prometheus |
| NETBOX01 | `10.10.20.14` | NetBox IPAM/source of truth |
| ANSIBLE01 | `10.10.20.15` | Ansible control node |
| WINCLIENT01 | `10.10.10.191` | Windows domain client |

## Technologies

**Networking**
- VLANs
- IEEE 802.1Q
- Inter-VLAN routing
- NAT
- DHCP
- DNS
- Firewall policies
- SNMP
- Syslog

**Virtualization & Systems**
- Proxmox VE
- Ubuntu Server
- Windows Server
- Active Directory Domain Services
- Group Policy
- OPNsense

**Monitoring**
- LibreNMS
- Grafana
- Prometheus
- Node Exporter
- Windows Exporter

**Infrastructure Management**
- NetBox
- Ansible
- Git/GitHub

## Active Directory

The lab uses the Active Directory domain:

`corp.example.test`

DC01 provides centralized authentication and DNS services. WINCLIENT01 is domain joined on the CLIENTS VLAN while DC01 resides on the SERVERS VLAN, requiring routed communication between the two network segments.

## Monitoring

The lab uses two complementary monitoring platforms.

### LibreNMS

LibreNMS provides SNMP-based monitoring and historical performance information for infrastructure systems.

### Grafana + Prometheus

Prometheus collects system metrics from Linux Node Exporter and Windows Exporter. Grafana provides visualization of:

- CPU utilization
- Memory utilization
- Disk utilization
- Network traffic
- System load
- Uptime

## Centralized Logging

OPNsense firewall events are forwarded to NMS01 using remote syslog.

This provides centralized visibility into permitted and blocked network traffic and creates a dedicated location for firewall-event troubleshooting.

## NetBox

NetBox acts as the infrastructure source of truth and documents:

- VLANs
- IP prefixes
- IP addresses
- Physical devices
- Virtual machines
- Interfaces
- Physical cabling
- Proxmox cluster resources

## Ansible Automation

ANSIBLE01 manages the Linux infrastructure using SSH key authentication.

Managed systems include:

- NMS01
- GRAFANA01
- NETBOX01

Ansible was validated using multi-host connectivity tests, automated fact gathering, infrastructure auditing, and remote system-state verification.

## Validation

The completed environment was validated through:

- Domain controller discovery
- Active Directory authentication
- DNS resolution
- Inter-VLAN connectivity
- Internet connectivity
- SNMP monitoring
- Prometheus metric collection
- Grafana dashboard updates
- Centralized OPNsense syslog
- NetBox IPAM and topology documentation
- Multi-host Ansible management

## Documentation

Additional technical documentation is available in the [`docs`](docs/) directory.

- [Architecture](docs/architecture.md)

## Skills Demonstrated

This project demonstrates hands-on experience with:

- Enterprise network segmentation
- Layer 2 and Layer 3 networking
- Windows domain administration
- Linux server administration
- Firewall and routing configuration
- Network and system monitoring
- Centralized logging
- Infrastructure documentation
- IP address management
- Configuration management
- Troubleshooting across network and system layers

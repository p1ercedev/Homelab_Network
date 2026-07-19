# Universe Homelab


<img width="1349" height="869" alt="homelab" src="https://github.com/user-attachments/assets/992630b3-c3d2-462f-968c-75f3294d40b5" />



## Network Overview

A multi-segment cyber range and homelab built for offensive security research, Active Directory attack simulation, blue team detection engineering, and infrastructure automation. Everything runs on-prem on Proxmox with isolated virtual bridges and a pfSense perimeter.

| Stat | Value |
|------|-------|
| Machines | 14 |
| AD Forests | 3 |
| Networks | 4 |
| Services | 20+ |

---

## All Machines

| Hostname | Role | Network | IP | Services / Notes |
|----------|------|---------|----|------------------|
| `pf01` | Firewall / Router | ALL | `.253` / `.15` | pfSense — routing, stateful inspection, NAT, VPN |
| `DC01.testlab.local` | Domain Controller | AD Lab (`192.168.1.0/24`) | `.10` | Primary DC for `testlab.local` — AD DS, DNS |
| `DC02.internal.testlab.local` | Domain Controller | AD Lab (`192.168.1.0/24`) | `.20` | Subdomain DC for `internal.testlab.local` — AD DS, DNS |
| `DC03.corelab.local` | Domain Controller | AD Lab (`192.168.1.0/24`) | `.30` | Core services forest DC for `corelab.local` — AD DS, DNS |
| `dev01` | Windows Client | AD Lab (`192.168.1.0/24`) | `.1` | Sysmon, Splunk Universal Forwarder — endpoint telemetry |
| `dev02` | Windows Client | AD Lab (`192.168.1.0/24`) | `.2` | Sysmon, Splunk Universal Forwarder — comparative testing |
| `mon01` | Wazuh | SOC (`192.168.100.0/28`) | `.1` | Wazuh manager — host-based detection & response |
| `mon02` | Splunk | SOC (`192.168.100.0/28`) | `.2` | Splunk Enterprise — centralized SIEM |
| `mon03` | Grafana Stack | SOC (`192.168.100.0/28`) | `.3` | Prometheus, Grafana, Netdata — metrics & observability |
| `nsm01` | Network Security Monitoring | SOC (`192.168.100.0/28`) | `.4` | Zeek & Suricata — NSM, PCAP, IDS alerts |
| `tkt01` | Incident Response | SOC (`192.168.100.0/28`) | `.5` | The Hive — case management & IR ticketing |
| `Web01` | Vulnerable Web App | AD Lab (via `vmbr2`) | `.240` | WebSploit — intentionally vulnerable web target |
| `linux01` | Automation / Containers | Linux (`192.168.10.0/24`) | — | Docker, K3s, Portainer, GitLab, Nginx reverse proxy |

---

## Network Topology

Four isolated segments interconnected through a pfSense perimeter. All inter-segment traffic routes through `pf01` — no flat network.

### Segments

| Segment | Subnet | Bridge | Purpose |
|---------|--------|--------|---------|
| **WAN** | `10.30.100.0/22` | `vmbr0` | Uplink / external connectivity |
| **SOC** | `192.168.100.0/28` | `vmbr1` | SIEM, NSM, observability, IR |
| **Active Directory Lab** | `192.168.1.0/24` | `vmbr2` | Domain controllers, clients, attack targets |
| **Linux** | `192.168.10.0/24` | `vmbr4` | Containers, automation, DevOps tooling |

### Virtual Bridges & Routing

- **vmbr0** — WAN uplink
- **vmbr1** — SOC network, connects to `pf01` at `.15`
- **vmbr2** — AD Lab + WebSploit, connects to `pf01` at `.253`
- **vmbr4** — Linux infrastructure
- **OVS-Mirror** — port mirror between SOC and AD Lab for passive network visibility (Zeek/Suricata tap)

---

## Core Network and Edge Security

### pf01

**Services:** pfSense routing, stateful firewall (inspection, NAT), VPN termination

**Notes:** Primary perimeter device for all lab networks. Bridges WAN, SOC, AD Lab, and Linux segments.

---

## Active Directory Lab

### Forests / Domains

- `testlab.local`
  - `internal.testlab.local`
- `corelab.local`

### Domain Controllers

#### DC01.testlab.local
**Services:** AD DS, DNS
**Notes:** Primary DC for `testlab.local`

#### DC02.internal.testlab.local
**Services:** AD DS, DNS
**Notes:** DC for `internal.testlab.local`

#### DC03.corelab.local
**Services:** AD DS, DNS
**Notes:** DC for `corelab.local`

### Windows Clients

#### dev01
**Services:** Sysmon, Splunk Universal Forwarder
**Notes:** Endpoint telemetry generation

#### dev02
**Services:** Sysmon, Splunk Universal Forwarder
**Notes:** Comparative endpoint testing

### Attack Targets

#### Web01
**Services:** WebSploit
**Notes:** Intentionally vulnerable web application for offensive testing

---

## SOC / SIEM / Blue Team

### mon01
**Services:** Wazuh
**Notes:** Host-based intrusion detection and response

### mon02
**Services:** Splunk Enterprise
**Notes:** Centralized SIEM and log aggregation

### nsm01
**Services:** Zeek, Suricata
**Notes:** Network security monitoring via OVS-Mirror tap

### tkt01
**Services:** The Hive
**Notes:** Incident response case management and ticketing

---

## Monitoring and Observability

### mon03
**Services:** Prometheus, Grafana, Netdata
**Notes:** Infrastructure metrics, dashboards, and real-time health monitoring

---

## Linux Infrastructure and Tooling

### linux01
**Services:** Docker, K3s, Portainer, GitLab, Nginx reverse proxy
**Notes:** Container hosting, CI/CD, and automation

---

## Current Add-ons

- Active Directory
  - SCCM

### Planned Services

- LimaCharlie EDR
- Velociraptor (server + Windows/Linux agents)

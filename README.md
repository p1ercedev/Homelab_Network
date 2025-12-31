# Universe Homelab
![Architecture Diagram](homelab-layout.png)

# All Machines
| Hostname  | Role                                       | OS              | Network(s)                | Key Tech From List                                  |
| --------- | ------------------------------------------ | --------------- | ------------------------- | --------------------------------------------------- |
| pf01      | Main FW/Router                             | pfSense         | vmbr0 (WAN), LAB-LAN, DMZ | OPNsense/pfSense, DHCP, basic firewall/VPN          |
| DC01      | Domain Controller (testlab.local)          | Windows Server  | LAB-LAN                   | Active Directory, DNS                               |
| DC02      | Domain Controller (internal.testlab.local) | Windows Server  | LAB-LAN                   | Active Directory, DNS                               |
| DC03      | Domain Controller (corelab.local)          | Windows Server  | LAB-LAN                   | Active Directory, DNS                               |
| dev01     | Windows client                             | Windows 10      | LAB-LAN                   | Sysmon, Splunk Universal Forwarder                  |
| dev02     | Windows client                             | Windows 11      | LAB-LAN                   | Sysmon, Splunk Universal Forwarder                  |
| **mon01** | Metrics + infra monitoring                 | Ubuntu LTS      | LAB-LAN                   | Prometheus, Grafana, Netdata                        |
| linux01   | Docker / K3s / Git / Ansible host          | Ubuntu LTS      | LAB-LAN, LAB-DMZ          | Docker, K3s, Portainer, GitLab, Nginx reverse proxy |
| so01      | Security Onion (NSM stack)                 | Ubuntu (SO ISO) | SPAN from pf01 + LAB-LAN  | Zeek, Suricata, Security Onion 2                    |

# Ideas
**Current Addons**
- Active Directory
	- SCCM

**New Services**
- LimaCharlie EDR
- Velociraptor server, Windows + Linux agents deployed

---

## Core Network and Edge Security

| Hostname | Role           | OS      | Network(s)                    | OS & Services        |
|----------|----------------|---------|-------------------------------|-------------------------------------|
| `pf01`   | Main FW/Router | pfSense | `vmbr0` (WAN), `LAB-LAN`, DMZ | pfSense, firewall |

---

## Active Directory Lab

### Forests / Domains

- `testlab.local`
  - `internal.testlab.local`
- `corelab.local`

### Domain Controllers

| Hostname                      | Role                            | OS             | Network(s) | Notes                        |
|-------------------------------|----------------------------------|----------------|-----------|------------------------------|
| `DC01.testlab.local`          | DC for `testlab.local`          | Windows Server | `LAB-LAN` | Primary testlab DC           |
| `DC02.internal.testlab.local` | DC for `internal.testlab.local` | Windows Server | `LAB-LAN` | Internal subdomain DC        |
| `DC03.corelab.local`          | DC for `corelab.local`          | Windows Server | `LAB-LAN` | Core services forest DC      |

### Windows Clients and Workstations

| Hostname | Role           | OS         | Network(s) | Key Tech From List                  |
|----------|----------------|------------|------------|-------------------------------------|
| `dev01`  | Windows client | Windows 10 | `LAB-LAN`  | Sysmon, Splunk Universal Forwarder  |
| `dev02`  | Windows client | Windows 11 | `LAB-LAN`  | Sysmon, Splunk Universal Forwarder  |

---

## SOC / SIEM / Blue Team

| Hostname | Role                      | OS         | Network(s) | Services                                                     |
|----------|---------------------------|------------|------------|------------------------------------------------------------------------------------|
| `mon01`  | Metrics + infra monitoring | Ubuntu LTS | 
| `so01`   | NSM / Security Onion      | Oracle Linux | AD-Lab, Linux, Lab | Zeek, Suricata, Security Onion stack (alerts, PCAP, dashboards) |

---

## Monitoring and Observability

| Hostname | Role                         | OS         | Network(s) | Services            |
|----------|------------------------------|------------|------------|----------------------------------------|
| `mon01`  | Metrics and infra monitoring | Ubuntu LTS | `LAB-LAN`  | Prometheus, Grafana, Netdata (SNMP/hosts) |

---

## Linux Infrastructure and Tooling

| Hostname | Role                           | OS         | Network(s)          | Services                      |
|----------|--------------------------------|------------|---------------------|--------------------------------------------------|
| `linux1` | Docker / K3s / Git / Ansible   | Ubuntu LTS | `LAB-LAN`, `LAB-DMZ`| Docker, K3s, Portainer, GitLab, Nginx reverse proxy |

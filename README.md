# Universe Homelab
![Architecture Diagram](homelab-layout.png)

# All Machines
| Hostname                      | Role                                         | Network(s)  | Services / Notes                                                |
| ----------------------------- | -------------------------------------------- | ----------- | --------------------------------------------------------------- |
| `pf01`                        | Main Firewall / Router                       | All         | pfSense routing, firewall, NAT, VPN                             |
| `DC01.testlab.local`          | Domain Controller (`testlab.local`)          | `LAB-LAN`   | Primary DC for testlab forest                                   |
| `DC02.internal.testlab.local` | Domain Controller (`internal.testlab.local`) | `LAB-LAN`   | Subdomain DC                                                    |
| `DC03.corelab.local`          | Domain Controller (`corelab.local`)          | `LAB-LAN`   | Core services forest DC                                         |
| `dev01`                       | Windows Client                               | `LAB-LAN`   | Sysmon, Splunk Universal Forwarder                              |
| `dev02`                       | Windows Client                               | `LAB-LAN`   | Sysmon, Splunk Universal Forwarder                              |
| `mon01`                       | SIEM & SOC Platform                          | `MGMT-LAN`  | Splunk Enterprise, Wazuh                                        |
| `so01`                        | Network Security Monitoring                  | `MGMT-LAN`  | Security Onion stack (Zeek, Suricata, alerts, PCAP, dashboards) |
| `mon02`                       | Infra & Metrics Monitoring                   | `MGMT-LAN`  | Prometheus, Grafana, Netdata, SNMP                              |
| `linux01`                     | Automation / Containers / DevOps             | `LINUX-LAN` | Docker, K3s, Portainer, GitLab, Nginx reverse proxy             |
# Ideas
**Current Addons**
- Active Directory
	- SCCM

**New Services**
- LimaCharlie EDR
- Velociraptor server, Windows + Linux agents deployed

---

## Core Network and Edge Security
| Hostname | Role           | OS      | Network(s) | Services               |
| -------- | -------------- | ------- | ---------- | ---------------------- |
| `pf01`   | Main FW/Router | pfSense | All        | pfSense routing and FW |

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

| Hostname | Role                 | OS            | Network(s) | Services                                                        |
| -------- | -------------------- | ------------- | ---------- | --------------------------------------------------------------- |
| `mon01`  | SIEM & SOC Tools     | Ubuntu Server | MGMT-LAN   | Splunk Enterprise, Wazuh                                        |
| `so01`   | NSM / Security Onion | Oracle Linux  | MGMT-LAN   | Zeek, Suricata, Security Onion stack (alerts, PCAP, dashboards) |

---

## Monitoring and Observability

| Hostname | Role                         | OS            | Network(s) | Services                                  |
| -------- | ---------------------------- | ------------- | ---------- | ----------------------------------------- |
| `mon02`  | Metrics and infra monitoring | Ubuntu LTS    | MGMT-LAN   | Prometheus, Grafana, Netdata (SNMP/hosts) |

---

## Linux Infrastructure and Tooling

| Hostname  | Role                              | OS         | Network(s) | Services                                            |
| --------- | --------------------------------- | ---------- | ---------- | --------------------------------------------------- |
| `linux01` | Docker / K3s / Git / Ansible host | Ubuntu LTS | LINUX-LAN  | Docker, K3s, Portainer, GitLab, Nginx reverse proxy |

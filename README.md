# Universe Homelab
![Architecture Diagram](homelab-layout.png)

# All Machines
| Hostname                      | Category         | Role                                         | OS             | Network(s)  | Services / Notes                                                |
| ----------------------------- | ---------------- | -------------------------------------------- | -------------- | ----------- | --------------------------------------------------------------- |
| `pf01`                        | Core Network     | Main Firewall / Router                       | pfSense        | All         | pfSense routing, firewall, NAT, VPN                             |
| `DC01.testlab.local`          | Active Directory | Domain Controller (`testlab.local`)          | Windows Server | `LAB-LAN`   | Primary DC for testlab forest                                   |
| `DC02.internal.testlab.local` | Active Directory | Domain Controller (`internal.testlab.local`) | Windows Server | `LAB-LAN`   | Subdomain DC                                                    |
| `DC03.corelab.local`          | Active Directory | Domain Controller (`corelab.local`)          | Windows Server | `LAB-LAN`   | Core services forest DC                                         |
| `dev01`                       | Windows Endpoint | Windows Client                               | Windows 10     | `LAB-LAN`   | Sysmon, Splunk Universal Forwarder                              |
| `dev02`                       | Windows Endpoint | Windows Client                               | Windows 11     | `LAB-LAN`   | Sysmon, Splunk Universal Forwarder                              |
| `mon01`                       | SOC / SIEM       | SIEM & SOC Platform                          | Ubuntu Server  | `MGMT-LAN`  | Splunk Enterprise, Wazuh                                        |
| `so01`                        | SOC / NSM        | Network Security Monitoring                  | Oracle Linux   | `MGMT-LAN`  | Security Onion stack (Zeek, Suricata, alerts, PCAP, dashboards) |
| `mon02`                       | Monitoring       | Infra & Metrics Monitoring                   | Ubuntu Serever | `MGMT-LAN`  | Prometheus, Grafana, Netdata, SNMP                              |
| `linux01`                     | Linux Infra      | Automation / Containers / DevOps             | Ubuntu Server  | `LINUX-LAN` | Docker, K3s, Portainer, GitLab, Nginx reverse proxy             |
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

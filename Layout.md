# Universe Homelab

![[homelab-layout.png]]


| Hostname | Role | OS  | Network(s) | Key Tech From List |
| -------- | ---- | --- | ---------- | ------------------ |
| `pf01`   | Main FW/Router | pfSense | `vmbr0` (WAN), `LAB-LAN`, DMZ | OPNsense, DHCP, basic firewall/VPN |
---

## AD Lab
- `testlab.local`
  - `internal.testlab.local`
- `corelab.local`

---

# Active Directory Lab

- **DC01.testlab.local**
- **DC02.internal.testlab.local**
- **DC03.corelab.local**

---

# Linux Lab

| Hostname | Role                      | OS         | Network(s) | Key Tech From List                                                              |
|----------|---------------------------|------------|------------|--------------------------------------------------------------------------------|
| `harbor` | Syslog + SIEM (Blue core) | Ubuntu LTS | `LAB-LAN`  | RSyslog/Syslog-ng, Wazuh or Elastic, Splunk (log rotation/compression?) Docker         |

---

# Windows Clients

| Hostname | Role           | OS         | Network(s) | Key Tech From List                  |
|----------|----------------|------------|------------|------------------------------------|
| `dev01`  | Windows client | Windows 10 | `LAB-LAN`  | Sysmon, Splunk Universal Forwarder |
| `dev02`  | Windows client | Windows 11 | `LAB-LAN`  | Sysmon, Splunk Universal Forwarder |

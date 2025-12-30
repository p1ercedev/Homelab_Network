# 🧰 Homelab Stack for Sysadmin → Cybersecurity

## ⚙️ Sysadmin Stack

### 🔐 Firewall & Proxy
- OPNsense
- Squidproxy

### 🐧 Linux Systems (RHCSA / Cyber Focus)
- CentOS
- RHEL
- REMnux
- OPNsense
- Ansible Control Node
- Docker Host / Swarm
  - K3s (lightweight Kubernetes)

### 🧪 Protocol & Application Servers
- Bind9 (DNS)
- Samba (File Sharing)
- Mail Server
  - Postfix + Dovecot
  - OR Zimbra / Mailcow / iRedMail
- Syslog Server (e.g., RSyslog or Syslog-ng)

### 🌐 Networking
- Failover DHCP (e.g., Kea DHCP)
- Bind DNS (internal zone management)
- RADIUS
  - FreeRADIUS
  - NPS Server (Windows)

### 📊 Monitoring & Logging
- Grafana + Prometheus
- Zabbix / LibreNMS / Netdata (SNMP Monitoring)

---

## 🛡️ Cybersecurity Stack

### 🔵 Blue Team
- Security Onion 2 (NSM Suite)
- IDS/IPS (Suricata / Zeek)
- Wazuh (with ELK)
- Sysmon + Winlogbeat → Wazuh/ELK
- Elastic Agent + Fleet Server
- Sigma Rules (Testing + Conversion)
- Splunk Free
- Graylog
- TheHive + Cortex (IR Management)
- Arkime (Moloch – Full Packet Capture)
- Velociraptor (DFIR + Threat Hunting)

### 🔴 Red Team
- VulnHub Boxes
- Parrot OS (Attack VM)
- C2 Frameworks (Dockerized)
  - Mythic, Covenant, Sliver, etc.

---

# 🧱 Recommended Additions (Sysadmin, Networking, Security)

## 🧑‍💼 Identity & Access Management
- Windows Server 2022 (Active Directory)
- FreeIPA or OpenLDAP (Linux directory services)
- Keycloak (SSO, OAuth2, OpenID Connect)

## 📡 Network Infrastructure & Routing
- pfSense (alternate to OPNsense)
- VyOS (CLI router/firewall for routing labs)
- MikroTik RouterOS (via CHR VM)
- FRRouting / BIRD (BGP/OSPF Labs)

## ☁️ Virtualization & Orchestration
- Proxmox VE or XCP-ng (Hypervisor environment)
- KVM + Cockpit (native Linux virtualization)
- Terraform (Infrastructure as Code)

## 🧪 Automation & Configuration Management
- SaltStack / Puppet (Ansible alternatives)
- HashiCorp Vault (secrets mgmt)
- Etcd / Consul (distributed configuration)

## 🗃️ File Sharing, NAS & Storage
- Nextcloud (self-hosted file cloud)
- TrueNAS CORE or SCALE (ZFS + iSCSI support)
- Ceph (distributed storage – advanced)

## ♻️ Backup & Recovery
- Veeam Community / UrBackup (Windows/Linux backup)
- BorgBackup / Restic (Linux backup automation)

## 🔍 Sysadmin Security Tools
- OSQuery (endpoint visibility)
- Auditd (Linux audit logging)
- ClamAV + Maldet (malware detection)
- Tripwire or AIDE (file integrity monitoring)

## 🧪 Network Testing & Simulation
- GNS3 or EVE-NG (network lab simulation)
- Wireshark + tcpreplay (PCAP analysis/replay)
- Netcat, iperf3, hping3 (connectivity + performance)

## 🧰 Admin Utilities
- Webmin / Cockpit (web-based Linux admin tools)
- Crontab / systemd timers (task automation)
- Logrotate (log file management)

---

# 🏗️ Homelab Structure Plan

## 🔹 Phase 0: Physical/Virtual Foundations
- Proxmox VE / XCP-ng / KVM
- Management: Cockpit, Portainer, Webmin
- Base VMs: Ubuntu, Rocky/AlmaLinux, Windows Server

## 🔹 Phase 1: Core Networking & Directory Services
- OPNsense/pfSense firewall
- Bind9, Failover DHCP
- FreeRADIUS / NPS
- Active Directory / FreeIPA

## 🔹 Phase 2: Infrastructure Services
- Samba, Mail Server
- Ansible, Git, Docker/K3s

## 🔹 Phase 3: Monitoring & Logging
- Grafana + Prometheus
- Netdata / Zabbix
- ELK / Wazuh / Graylog / Splunk

## 🔹 Phase 4: Blue/Red Teaming Tools
- Security Onion, Velociraptor, TheHive, Cortex
- Parrot OS, VulnHub, C2 Frameworks

## 🔹 Phase 5: Cloud Integration
- AWS/GCP/Azure Free Tier
- EC2, S3, IAM, CloudWatch
- VPN/VPC peering to homelab
- Cloud → SIEM log forwarding
- Terraform + GitOps for provisioning

---

## ☁️ Cloud Integration Use Cases

| Cloud Service | Lab Use Case |
|---------------|--------------|
| **EC2 Instances** | Host DNS, RADIUS, AD replicas, honeypots |
| **S3 Buckets** | Offsite backup target, data dump |
| **IAM & Roles** | Access control, audit trails |
| **CloudWatch / Azure Monitor** | Log forwarding from on-prem |
| **Cloudflare Tunnel** | Securely expose internal services |
| **Lambda / Azure Functions** | Alerting/automation from logs |
| **VPC Peering / VPN** | Secure hybrid network |

## ✅ Summary Table

| Phase | Purpose | Outcome |
|-------|---------|---------|
| 0 | Hypervisor/VM foundation | Stable, scalable core |
| 1 | Networking + Identity | Full LAN auth domain |
| 2 | Infrastructure services | Enterprise-like env |
| 3 | Monitoring/Logging | Performance + Security visibility |
| 4 | Blue/Red Team tools | Offense + defense labs |
| 5 | Cloud hybrid | Real-world hybrid/enterprise simulation |
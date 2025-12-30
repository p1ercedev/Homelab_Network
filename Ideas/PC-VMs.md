- Sys Admin Stuff
  - 
        - Linux Boxes (RHCSA/Cyber)
                - CentOS
                - REHL
                - rEMnux
                - OPNsense
                - Ansible Control Node
                - Docker Host/Swarm
                        - K3s
                - Protocol Server 
                        - Bind9, 
                        - Samba
                        - Mail Server Postfix + Dovecot | Zimbra / Mailcow / iRedMail
                        - Syslog 

                - Networking
                        - Failover DHCP
                        - Bind DNS
                        - Radius + FreeRADIUS / NPS Serevr

                - Monitoring & Logging 
                        - Grafana + Prometheus
                        - Zabbix / LibreNMS / Netdata (SNMP)



- Cyber Stuff
        - Blue Team 
           - Security Onion 2
           - IDS/IPS
           - Wazuh
                - Sysmon + Winlogbeat → ELK/Wazuh
           - Elastic Agent + Fleet Server
           - Sigma Rules Testing
           - Splunk Free
           - Graylog
           - TheHive + Cortex
           - Arkime (Moloch)
           - Velociraptor 

        - Red Team 
                - VulnHub Boxes
                - Parrot OS
                - C2 Server (dockers)


                     [You: Laptop / Phone]
                            |
                       (WireGuard VPN)
                            |
                     [OPNsense Firewall]
                        /          \
         [Mgmt LAN/VLAN]         [Homelab LAN/VLAN]
           (RustDesk Host)           (All VMs)
               |
        [Access via GUI or CLI]

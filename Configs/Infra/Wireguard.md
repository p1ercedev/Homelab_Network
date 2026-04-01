**wg-helper script (dashboard on port 10086)**

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/wireguard.sh)"
```

```bash
p1erce@mon01:~$ sudo ip route replace 192.168.1.0/24 via 192.168.100.13 dev ens19
I also ran pfctl -d temporarily to allow for ICMP
```

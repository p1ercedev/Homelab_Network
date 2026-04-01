```yml
services:
  zeek:
    container_name: zeek
    image: activecm/zeek
    network_mode: host
    restart: always
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /root/zeek-spool:/usr/local/zeek/spool/
      - /root/zeek-logs:/usr/local/zeek/logs/
      - /root/node.cfg:/usr/local/zeek/etc/node.cfg
    cap_add:
    - NET_RAW
    - NET_ADMIN
```
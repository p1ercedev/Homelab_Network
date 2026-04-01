```yml
services:
  zeek:
    container_name: zeek
    image: activecm/zeek
    network_mode: host
    restart: always
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /root/zeek/zeek-spool:/usr/local/zeek/spool/
      - /root/zeek/zeek-logs:/usr/local/zeek/logs/
      - /root/zeek/node.cfg:/usr/local/zeek/etc/node.cfg
    cap_add:
    - NET_RAW
    - NET_ADMIN

  suricata:
    container_name: suricata
    image: jasonish/suricata
    network_mode: host
    restart: always
    volumes:
      - /root/suricata/logs:/var/log/suricata
      - /root/suricata/rules:/var/lib/suricata
      - /root/suricata/etc:/etc/suricata
    environment:
      - ENABLE_CRON=yes
      - SURICATA_OPTIONS=-i eth1 -vvv
    cap_add:
    - NET_RAW
    - NET_ADMIN
    -
```

```bash
root@nsm01:~/suricata# docker exec -it --user suricata suricata suricata-update -f
1/4/2026 -- 02:20:40 - <Info> -- Loading /etc/suricata/update.yaml
1/4/2026 -- 02:20:40 - <Info> -- Using data-directory /var/lib/suricata.
1/4/2026 -- 02:20:40 - <Info> -- Using Suricata configuration /etc/suricata/suricata.yaml
1/4/2026 -- 02:20:40 - <Info> -- Using /usr/share/suricata/rules for Suricata provided rules.
1/4/2026 -- 02:20:40 - <Info> -- Found Suricata version 8.0.4 at /usr/bin/suricata.
1/4/2026 -- 02:20:40 - <Info> -- Loading /etc/suricata/suricata.yaml
1/4/2026 -- 02:20:40 - <Info> -- Disabling rules for protocol pgsql
1/4/2026 -- 02:20:40 - <Info> -- Disabling rules for protocol modbus
1/4/2026 -- 02:20:40 - <Info> -- Disabling rules for protocol dnp3
1/4/2026 -- 02:20:40 - <Info> -- Disabling rules for protocol enip
1/4/2026 -- 02:20:40 - <Info> -- No sources configured, will use Emerging Threats Open
1/4/2026 -- 02:20:40 - <Info> -- Fetching https://rules.emergingthreats.net/open/suricata-8.0.4/emerging.rules.tar.gz.
 100% - 5407694/5407694               
1/4/2026 -- 02:20:41 - <Info> -- Done.
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/app-layer-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/decoder-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/dhcp-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/dnp3-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/dns-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/files.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/http2-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/http-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/ipsec-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/kerberos-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/modbus-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/mqtt-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/nfs-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/ntp-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/quic-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/rfb-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/smb-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/smtp-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/ssh-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/stream-events.rules
1/4/2026 -- 02:20:41 - <Info> -- Loading distribution rule file /usr/share/suricata/rules/tls-events.rules
1/4/2026 -- 02:20:42 - <Info> -- Ignoring file e8e18dbaadbcd7eebb54ecdb5c78f603/rules/emerging-deleted.rules
1/4/2026 -- 02:20:45 - <Info> -- Loaded 65226 rules.
1/4/2026 -- 02:20:46 - <Info> -- Disabled 15 rules.
1/4/2026 -- 02:20:46 - <Info> -- Enabled 0 rules.
1/4/2026 -- 02:20:46 - <Info> -- Modified 0 rules.
1/4/2026 -- 02:20:46 - <Info> -- Dropped 0 rules.
1/4/2026 -- 02:20:46 - <Info> -- Enabled 136 rules for flowbit dependencies.
1/4/2026 -- 02:20:46 - <Info> -- Creating directory /var/lib/suricata/rules.
1/4/2026 -- 02:20:46 - <Info> -- Backing up current rules.
1/4/2026 -- 02:20:46 - <Info> -- Writing rules to /var/lib/suricata/rules/suricata.rules: total: 65226; enabled: 49361; added: 65226; removed 0; modified: 0
1/4/2026 -- 02:20:46 - <Info> -- Writing /var/lib/suricata/rules/classification.config
1/4/2026 -- 02:20:46 - <Info> -- Testing with suricata -T.

1/4/2026 -- 02:21:23 - <Info> -- Running suricatasc -c reload-rules.
1/4/2026 -- 02:21:39 - <Info> -- Reload command returned: {"message":"done","return":"OK"}

1/4/2026 -- 02:21:39 - <Info> -- Done.
```

**Changes to suricata.yml**

I had to specify this directly maybe cuz it's an LXC
```yaml
af-packet:
  - interface: eth0
    # Number of receive threads. "auto" uses the number of cores 
    threads: 4
```

And here too

```yaml
  # DPDK capture support
  # RX queues (and TX queues in IPS mode) are assigned to cores in 1:1 ratio
  interfaces:
    - interface: 0000:3b:00.0 # PCIe address of the NIC port
      # Threading: possible values are either "auto" or number of threads
      # - auto takes all cores
      # in IPS mode it is required to specify the number of cores and the numbers on both interfaces must match
      threads: 4
```


And here too
```yaml
# PF_RING configuration: for use with native PF_RING support
# for more info see http://www.ntop.org/products/pf_ring/
pfring:
  - interface: eth0
    # Number of receive threads. If set to 'auto' Suricata will first try
    # to use CPU (core) count and otherwise RSS queue count.
    threads: 4
```

**Adding Payload to alert**

I also wanted to be able to see the exact payload run which is in the suricata.yml (line 179)

```yaml
  types:
        - alert:
            #payload: yes             # enable dumping payload in Base64
            # payload-buffer-size: 4 KiB  # max size of payload buffer to output in eve-log
            #payload-printable: yes   # enable dumping payload in printable (lossy) format
            # payload-length: yes      # enable dumping payload length, including the gaps
            # packet: yes              # enable dumping of packet (without stream segments)
            # metadata: no             # enable inclusion of app layer metadata with alert. Default yes
            # If you want metadata, use:
            metadata:
              # Include the decoded application layer (ie. http, dns)
              app-layer: true
              # Log the current state of the flow record.
              flow: true
              rule:
                # Log the metadata field from the rule in a structured format.
                metadata: true
                # Log the raw rule text.
                raw: false
                reference: false      # include reference information from the rule
            #http-body: yes           # Requires metadata; enable dumping of HTTP body in Base64
            http-body-printable: yes # Requires metadata; enable dumping of HTTP body in printable format
            # websocket-payload: yes   # Requires metadata; enable dumping of WebSocket Payload in Base64
            # websocket-payload-printable: yes # Requires metadata; enable dumping of WebSocket Payload in printable format

            # Enable the logging of tagged packets for rules using the
            # "tag" keyword.
            tagged-packets: yes
            # Enable logging the final action taken on a packet by the engine
            # (e.g: the alert may have action 'allowed' but the verdict be
            # 'drop' due to another alert. That's the engine's verdict)
            # verdict: yes
        # app layer frames
```


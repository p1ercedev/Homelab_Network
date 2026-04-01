**Update suricata file**

```bash
root@nsm01:~/suricata# docker exec -it --user suricata suricata suricata-update -f
<SNIP>
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


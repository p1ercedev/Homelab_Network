# OVS sniffing port with LXC

`tc` failed so I switched to OVS. I make vmbr2 a OVS and then have it mirrored to my device (my LXC container running Zeek and Suricata). I assigned the virtual network interface on the container. This is possible b/c every device connected gets a virtual network interface created on the host side (like tap100i0 or veth100i0), so I assigned the mirror running on `vmbr2` to output to `veth100i1` on the LXC.

I had to shutdown before and then reboot after the interface was changed

**/etc/network/interfaces**

```bash
auto vmbr2
iface vmbr2 inet static
        address 192.168.1.254/24
        ovs_type OVSBridge
        ovs_bridge none
```

```bash
brctl show vmbr2
```

```bash
ovs-vsctl -- --id=@p get port veth100i1 -- --id=@m create mirror name=m0 select-all=true output-port=@p -- set bridge vmbr2 mirrors=@m   
```

```bash
ovs-vsctl  list Mirror m0
```

```bash
root@nsm01:~# touch node.cfg
root@nsm01:~# docker run --rm -it --network host \
    --mount source=$(pwd)/node.cfg,destination=/node.cfg,type=bind \
    activecm/zeek \
    zeekcfg -o /node.cfg --type afpacket
Unable to find image 'activecm/zeek:latest' locally
latest: Pulling from activecm/zeek
78970462799d: Pull complete
e5039733dcb1: Pull complete
d7de1aa9deb9: Pull complete
dcc1106f9ba0: Pull complete
1038faf51ad4: Pull complete
efe62d08a4b9: Pull complete
5404b71574c4: Pull complete
daad43d08496: Pull complete
e81b61f6a329: Pull complete
fc6a0c26252e: Pull complete
a45bbb1c998e: Pull complete
c6a83fedfae6: Pull complete
7a4964c4567e: Download complete
Digest: sha256:c3d4f319b7f1840006c3bd2b0898be36e3306717c4b4a7cf39baa870427ad572
Status: Downloaded newer image for activecm/zeek:latest
? Choose your capture interface(s): eth1
? How many total Zeek processes do you want? 3
? Would you like to pin Zeek worker processes to specific CPUs? No
```

```bash
root@nsm01:~# more node.cfg
[manager]
type=manager
host=localhost

[proxy-1]
type=proxy
host=localhost

[worker-eth1]
type=worker
host=localhost
# See https://github.com/J-Gras/zeek-af_packet-plugin for plugin installation and further configuration
interface=af_packet::eth1
lb_procs=1
lb_method=custom
af_packet_fanout_id=0
af_packet_fanout_mode=AF_Packet::FANOUT_HASH
af_packet_buffer_size=128*1024*1024
```

```bash

```

```bash

```

# OVS sniffing port with LXC

`tc` failed so I switched to OVS. I make vmbr2 a OVS and then have it mirrored to my device (my LXC container running Zeek and Suricata). I assigned the virtual network interface on the container. This is possible b/c every device connected gets a virtual network interface created on the host side (like tap100i0 or veth100i0), so I assigned the mirror running on `vmbr2` to output to `veth100i1` on the LXC.

I had to shutdown before and then reboot after the interface was changed

**I had to shut down all CTs and VMs attached to the bridge (if linux bridge)**

```bash
brctl show vmbr2
```

`Shut everything down`

**/etc/network/interfaces**

```bash
auto vmbr2
iface vmbr2 inet static
        address 192.168.1.254/24
        ovs_type OVSBridge
        ovs_bridge none
```

**Get virtual interface name you want to mirror to**

```bash
root@pxmx:~# ovs-vsctl show
17664203-e56f-4da2-9f7c-f9b898572904
    Bridge vmbr2
        <SNIP>
        Port veth100i1
            Interface veth100i1
        <SNIP>
```

**Setup Mirror (does NOT persist through reboot)**

```bash
ovs-vsctl -- --id=@p get port veth100i1 -- --id=@m create mirror name=m0 select-all=true output-port=@p -- set bridge vmbr2 mirrors=@m   
```

**Check mirror status**

```bash
ovs-vsctl  list Mirror m0
```
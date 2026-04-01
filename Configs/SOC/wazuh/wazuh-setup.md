**Change cap settings and apparmor on LXC**

```bash
root@pxmx:~# cat /etc/pve/lxc/117.conf 
<SNIP>
lxc.apparmor.profile: unconfined
lxc.cgroup2.devices.allow: a
lxc.cap.drop: 
```

Otherwise the docs were good enough to help me

https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html
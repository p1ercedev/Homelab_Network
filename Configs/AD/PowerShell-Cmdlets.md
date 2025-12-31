**Dual Homed Machine to route to other subnets**

There's probably a better way to do this.
```powershell
New-NetRoute -DestinationPrefix 192.168.100.0/28 -InterfaceAlias "Ethernet" -NextHop 192.168.1.253
```
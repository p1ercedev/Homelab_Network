**Setup Splunk Forwarder**
This will forward saved logs to server
```powershell
PSKC:\indows\system32> "C: \Program Files \SplunkUniversalforwarder\etc\apps\search\local\inputs.conf"
[monitor://F: \sherlocklogs]
disabled = false
index = sherlock
```

**Configure Forwarder Server**
```powershell
PS C: \Windows\system32> cat "C:\Program Files\SplunkUniversalForwarder\etc\system\local\outputs.conf"
[tcpout:default-autolb-group]
disabled = false
server = 192.168.1.20:9997
[tcpout-server://192.168.1.2:9997]
```
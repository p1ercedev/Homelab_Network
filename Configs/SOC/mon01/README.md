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


# Grafana Stack
## Setting up influxdb2 with telegraf

### Things I learned
- Docker compose secrets
- The order in which you prune mattters (obvious but I thought I was removing the volumes when I ran down, so my configs would be messed up)

**docker-compose**
Telegraf needs access to write to the influxdb using an API token which we set in the telegraf.conf
We also need to specify the org and the bucket in use

```bash

services:
 
<SNIP>

  influxdb2:
    image: influxdb:2
    container_name: influxdb2
    ports:
      - 8086:8086
    env_file:
      - ./influx.env
    secrets:
      - influxdb2-admin-username
      - influxdb2-admin-password
      - influxdb2-admin-token
    volumes:
      - type: volume
        source: influxdb2-data
        target: /var/lib/influxdb2
      - type: volume
        source: influxdb2-config
        target: /etc/influxdb2
<SNIP>
  telegraf:
    image: telegraf:latest
    container_name: telegraf
    restart: unless-stopped
    user: "1000:988"
    env_file:
      - ./telegraf.env
    volumes:
      - '/:/hostfs:ro' # to monitor docker-vm
      - './telegraf.conf:/etc/telegraf/telegraf.conf:ro'
      - '/var/run/docker.sock:/var/run/docker.sock:ro'
    depends_on:
      - influxdb2
<SNIP>

I found this 

https://www.elastic.co/docs/reference/integrations/suricata


**Enable Nesting for LXC and confirm apparmor status**

```bash
root@pxmx:~# cat /etc/pve/lxc/100.conf 
cores: 4
features: nesting=1
<SNIP>
lxc.apparmor.profile: unconfined
```

The docker container (even for this version) failed as well as the apt-get. The solution that worked was downlaoding the OSS directly cuz LXC.

```bash
cd /opt
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-oss-7.10.2-linux-x86_64.tar.gz
tar xzf filebeat-oss-7.10.2-linux-x86_64.tar.gz
```



**Download wazuh template**

```bash
curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/v4.14.4/extensions/elasticsearch/7.x/wazuh-template.json
chmod go+r /etc/filebeat/wazuh-template.json
```

**Add credentials for `filebeat.yml` (you can't hardcode these you must add them as secrets)**

```bash
echo USER | ./filebeat keystore add username --stdin --force
echo PASS | ./filebeat keystore add password --stdin --force
```


**Test current config (note the current yml does NOT use the suricata and zeek modules)**

```bash
./filebeat test config -c /etc/filebeat/filebeat.yml
./filebeat test output -c /etc/filebeat/filebeat.yml
```

**Copy wazuh_certs (optional, but my template has this verification disabled.)**

```bash
mkdir -p /etc/filebeat/certs
cp wazuh_indexer_ssl_certs/wazuh.manager.pem /etc/filebeat/certs/filebeat.pem
cp wazuh_indexer_ssl_certs/wazuh.manager-key.pem /etc/filebeat/certs/filebeat-key.pem
cp wazuh_indexer_ssl_certs/root-ca.pem /etc/filebeat/certs/
chmod 500 /etc/filebeat/certs
chmod 400 /etc/filebeat/certs/*
chown -R root:root /etc/filebeat/certs
```
**Start filebeat**

```bash
cd /opt/filebeat-7.10.2-linux-x86_64
#./filebeat -e -c /etc/filebeat/filebeat.yml 2>&1 | head -80
./filebeat -e -c /etc/filebeat/filebeat.yml -d "publish" 2>&1 | head -200
```
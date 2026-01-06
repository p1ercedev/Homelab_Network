# Zeek
docker run -it --name zeek   -v "$(pwd)":/pcap   zeek/zeek:latest /bin/bash
## Windows7 Winbeat forwarder download

wget <https://artifacts.elastic.co/downloads/beats/winlogbeat/winlogbeat-7.17.16-windows-x86_64.zip>

## logstash.conf changes

input{
    beats {
        port => 5044
      }
}

output{
    elasticsearch{
        index => "winlogbeat-%{+YYYY.MM.dd}
    }
}

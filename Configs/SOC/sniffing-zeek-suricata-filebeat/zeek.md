
**Genreate default node.cfg**

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
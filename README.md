# Proxmox Quick Fix

## RRDC RRD Time related:
```
[status] notice: RRDC update error
[status] notice: RRD update error
---
systemctl stop rrdcached
cd /var/lib/
mv rrdcached rrdcached.bck
systemctl start rrdcached
systemctl restart pve-cluster
```


## Failed to start zfs import service during start up
### Problem due to start up before hdd spin up
```
Failed to start zfs-import@[pool name].service - Import ZFS pool [pool name].
---
systemctl edit zfs-import-cache.service
//insert
[Service]
ExecStartPre=/usr/bin/sleep 15
//save and restart
---
source:
https://ounapuu.ee/posts/2021/02/01/how-to-fix-zfs-pool-not-importing-at-boot/
```

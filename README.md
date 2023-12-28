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


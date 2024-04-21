# Proxmox Quick Fix

## Useful tips
#### Reading memory(ram) information
```
dmidecode --type memory
```
---
#### proxmox hostname has no internet, guest vm, internet working; misconfigured, try change to dhcp instead of static
```
vi /etc/network/interfaces
// update
auto vmbr0
iface vmbr0 inet dhcp
        bridge-ports enp2s0
        bridge-stp off
        bridge-fd 0
// save quit
systemctl restart networking
```
#### Monitor ECC memory checking for error
```
apt install rasdaemon
systemctl status rasdaemon
ras-mc-ctl --error-count
```


---
## RRDC RRD Time related:
#### Error in journalctl
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
#### Problem due to start up before hdd spin up
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

## kernel: EDID block 0 is all zeroes Error message
#### syslog flooded with EDID block 0 error
```
sudo -i
# new file
/etc/modprobe.d/igpu.conf
# enter in the file
blacklist i915
# save quit restart
```



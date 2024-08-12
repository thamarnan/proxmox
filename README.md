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

---

#### Monitor ECC memory checking for error
```
apt install rasdaemon
systemctl status rasdaemon
ras-mc-ctl --error-count
```


---
#### RRDC RRD Time related:
##### Error in journalctl
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
---
#### Failed to start zfs import service during start up
##### Problem due to start up before hdd spin up
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

#### kernel: EDID block 0 is all zeroes Error message
##### syslog flooded with EDID block 0 error
```
sudo -i
# new file
/etc/modprobe.d/igpu.conf
# enter in the file
blacklist i915
# save quit restart
```

#### no network after update
##### unable to access web ui or no network connection at all after apt update
```
login via console
pveversion -v | grep "not correctly"
some might show not correctly installed
try
dpkg --configure -a
to auto resolve
or
installed individual from cache
If there is a local cached version of the package (i.e. if ls -lah /var/cache/apt/archives/ifupdown2* returns a file), you can try re-installing that using apt install -f /var/cache/apt/archives/ifupdown[..].deb.
then reboot

source:
https://forum.proxmox.com/threads/network-state-down-after-update-reboot.130708/
```
---
#### banner on console show wrong ip
##### ip change but banner show old or different ip
```
edit host file
vi /etc/hosts

edit banner
/etc/issue

```
---
#### Check status active task from command
```
cd /var/log/pve/tasks
cat active
#see ending hex, eg, this is 4
UPID:xprox:003052C4:0FC6E037:661DC404:qmstart:101:root@pam:
cd 4
ls -tlr
cat UPID:xprox:003052C4:0FC6E037:661DC404:qmstart:101:root@pam:
```



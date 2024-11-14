# Collection of usefull tools for RPI


## ThermalCamera

https://makersportal.com/blog/2020/6/8/high-resolution-thermal-camera-with-raspberry-pi-and-mlx90640

apt-get install -y i2c-tools idle3



## Share the bridged network to the USB adapters

### Attach the devices to the bridge br0 on plugging

automatically add usb eth* devices to the bridge with the name `br0`

```
ACTION=="add", SUBSYSTEMS=="usb", SUBSYSTEM=="net", ATTR{type}=="1", \
    RUN{program}+="/usr/sbin/ip link set %k up", \
    RUN{program}+="/usr/sbin/ip link set %k master br0"    
```


### Disable DHCP for the adapters

disable dhcp on `eth1`

change the file: `/etc/dhcpcd.conf`

```
# RaspAP br0 configuration
denyinterfaces eth0 wlan0 eth1 eth2 eth3 eth4 eth5 eth6 eth7 eth8 eth9
interface br0
```

### Create systemd-networkd rule

to attach the interface to the bridge after the restart of the system:

`/etc/systemd/network/raspap-br0-member-eth1.network`

```
[Match]
Name=eth1

[Network]
Bridge=br0

```


### Fix the problem wit non-unique mac-adress of the bridge after the cloning of the SD-Card

this step is necessary if you cloned the SD-card to get unique Mac-Address for the existing network bridge

I could verify that the same bridge name would get a different "stable random" value after doing the operations described in Debian's link:

```
rm -f /etc/machine-id /var/lib/dbus/machine-id
dbus-uuidgen --ensure=/etc/machine-id
dbus-uuidgen --ensure
```







## Automatically mount usb drives

udev rule to automatically mount and unmount usb drives into the folder `/mnt`
https://wiki.archlinux.org/title/Udev#Mounting_drives_in_rules

#### Tested on:
1. manjaro
2. debian 12

```
ACTION=="add", KERNEL=="sd[a-z][0-9]|sd[a-z]", SUBSYSTEMS=="usb", SUBSYSTEM=="block", ENV{ID_FS_USAGE}=="filesystem", \
    RUN{program}+="/usr/bin/rm --dir --one-file-system --force --verbose /mnt/sd*", \
    RUN{program}+="/usr/bin/systemd-mount --owner=nobody --no-block --automount=yes  --timeout-idle-sec=90sec  --bind-device --options=rw,umask=000 --collect $devnode /mnt/%k"
```





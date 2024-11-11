# Collection of usefull tools for RPI


## ThermalCamera

https://makersportal.com/blog/2020/6/8/high-resolution-thermal-camera-with-raspberry-pi-and-mlx90640

apt-get install -y i2c-tools idle3




## Udev-rules

udev rule to automatically mount and unmount usb drives into the folder `/mnt`
https://wiki.archlinux.org/title/Udev#Mounting_drives_in_rules

#### Tested on:
1. manjaro
2. debian 12



```rules

ACTION=="add", KERNEL=="sd[a-z][0-9]", SUBSYSTEMS=="usb", SUBSYSTEM=="block", ENV{ID_FS_USAGE}=="filesystem", \
    RUN{program}+="/usr/bin/rm --dir --one-file-system --force --verbose /mnt/sd* " , \
    RUN{program}+="/usr/bin/systemd-mount --no-block --automount=yes --bind-device --options=rw,nouser --collect $devnode /mnt/%k" 

```



#### Bridge mac-address

I could verify that the same bridge name would get a different "stable random" value after doing the operations described in Debian's link:

```

rm -f /etc/machine-id /var/lib/dbus/machine-id
dbus-uuidgen --ensure=/etc/machine-id
dbus-uuidgen --ensure


```


#### ADD eth* to the `br0`

automatically add usb eth* devices to the bridge with the name `br0`


```
ACTION=="add", KERNEL=="eth[1-9]", SUBSYSTEMS=="usb", SUBSYSTEM=="net", ATTR{type}=="1", \
    RUN{program}+="/usr/sbin/ip link set eth1 master br0", \
    RUN{program}+="/usr/sbin/ip link set %k down", \
    RUN{program}+="/usr/sbin/ip link set %k up"


```




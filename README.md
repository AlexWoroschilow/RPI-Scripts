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

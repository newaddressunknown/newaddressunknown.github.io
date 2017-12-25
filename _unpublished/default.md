---
title: 'RasPiZero as USB multitool'
published: false
date: '23-11-2016 18:14'
taxonomy:
    category:
        - blog
    tag:
        - arch
        - linux
        - ssh
        - tmux
        - weechat
        - raspberrypi
external_links:
    process: true
    title: false
    no_follow: true
    target: _blank
    mode: active
mathjax:
    process: false
---

#### 1. Objective: 
Use the wonderufl PiZero as a usb multitool
Setup: Raspberry Pi Zero , arch arm sdcard
#### 2. Realisation:
pacman -Sy nano atom openssh wget curl
---------------------
configure sudo
export EDITOR=nano visudo
su 
visudo
add
alarm ALL=(ALL) ALL
---------------------
confiqure iot_phat
check version

scp ./eepflash.sh alarm@192.168.7.3:/home/alarm
--> update

    $ sudo nano /boot/config.txt
    add dtparam=i2c_vc=on to the end
    $ sudo reboot
chmod +x eepflash.sh
[alarm@alarmpi ~]$ sudo ./eepflash.sh -f=IoT_pHAT-with-dt.eep -t=24c32 -w
[sudo] password for alarm: 
This will disable the camera so you will need to REBOOT after this process completes.
This will attempt to write to i2c address 0x50. Make sure there is an eeprom at this address.
This script comes with ABSOLUTELY no warranty. Continue only if you know what you are doing.
Do you wish to continue? (yes/no): yes
Writing...
6+1 records in
6+1 records out
3344 bytes (3.3 kB, 3.3 KiB) copied, 66.8465 s, 0.1 kB/s
Done.
[alarm@alarmpi ~]$ 

done
---------------------
optimize zero for headless use
If you're running a headless Raspberry Pi, there's no need to power the display circuitry, and you can save a little power by running /usr/bin/tvservice -o (-p to re-enable). Add the line to /etc/rc.local to disable HDMI on boot.
done
---------------------
echo -e "interface usb0 \nstatic ip_address=169.254.64.64" | sudo tee -a /etc/dhcpcd.conf

systemctl start sshdgenkeys.service   

http://blog.iopsl.com/regenerate-openssh-host-keys-using-ssh-keygen/

systemctl start sshdkeygen



#### 3. Outlook:
https://pinout.xyz/pinout/zero_lipo#
https://pinout.xyz/pinout/iot_phat#
https://pinout.xyz/pinout/scroll_phat#

#### 4. Sources:
¹https://medium.com/@dwilkins/usb-gadget-mode-with-arch-linux-and-the-raspberry-pi-zero-e70a0f17730a
http://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/
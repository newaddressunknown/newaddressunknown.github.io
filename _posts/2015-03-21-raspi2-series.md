---
layout: post
title: RasPi2 Series I - Install ArchARM + First Steps
category: blog
published: true
date: '21-03-2015 13:34'
taxonomy:
    category:
        - blog
    tag:
        - arch
        - linux
        - arm
---
<!--excerpt-->
#### 1. Objective: 
Install ALARM - Arch Linux ARM on the raspberry 2, change SSH settings, change network (static ip)
Setup: Lenovo Yoga 13 (2012) Arch
Hardware Requirements: Raspberry Pi 2, MicroSD Card
#### 2. Realisation:
* Read and work through this guide¹.
Command not found error when formatting --> check dosfstools package status
* SSH to your RasPi2. The ip can usually be found in your router entries.        
`$ ssh root@<RasPi2_ip\>`
* Change root password        
`$ passwd`
* Change SSH port
Why? --> Yes, if someone does a portscan on your system a changed port is of little help ². Still, many attackers seem to routinely look for open ssh systems on port 22.
`$ nano /etc/ssh/sshd_config`
then change #Port 22 to
Port  <yourportnumber>
* Log out and try to login with
`$ ssh root@<raspi2_ip> -p  <yourportnumber>`
* Update the system and install necessary software
`$ pacman -Syu`
`$ pacman -Sy screen wget base-devel unzip`
* Create additional user
`$ useradd -m -G wheel, power -s /bin/bash <username>`
-G (groups) wheel for using sudo, power to reboot the device / -m (user directory) creates user directory at /home/<username> / -s (shell) usually bash
Change new user password
`$ passwd <username>`
Add user group wheel to sudoers file
You have to do this as root
`$ nano /etc/sudoers ALL=(ALL) ALL)`
Uncomment user group wheel
Reboot and login as <username>
* Install yaourt from source ³
Check that base-devel is installed
`$ sudo pacman -Sy base-devel`
First install package-query (yaourt dependency)
`$ wget https://aur.archlinux.org/packages/pa/package-query/package-query.tar.gz`
`$ tar -xvzf package-query.tar.gz`
`$ cd package-query`
`$ makepkg -si`
Then install yaourt 
Move to upper folder 
`$ cd ../ `
`$ wget https://aur.archlinux.org/packages/ya/yaourt/yaourt.tar.gz`
`$ tar -xvzf yaourt.tar.gz`
`$ cd yaourt`
`$ makepkg -si`

#### 3. Sources:
¹ http://archlinuxarm.org/platforms/armv7/broadcom/raspberry-pi-2
² https://www.adayinthelifeof.nl/2012/03/12/why-putting-ssh-on-another-port-than-22-is-bad-idea/
³ https://wiki.archlinux.de/title/yaourt
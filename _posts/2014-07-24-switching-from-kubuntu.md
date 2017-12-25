---
layout: post
title: Switching from Kubuntu 14.04 to Manjaro 0.8.10
category: blog
published: true
date: '24-07-2014 14:55'
taxonomy:
    category:
        - blog
---
<!--excerpt-->
#### 1. Initial situation:
Lenovo Yoga 13 (2012) with Kubuntu 14.04 LTS, first hard drive sda=Crucial M4 CT256, second hard drive sdb=Crucial CT480M
Previous knowledge: Creating a bootable USB drive, basic Linux shell commands
Tools: unetbootin
#### 2. Objective: 
Lenovo Yoga 13 (2012) with Manjaro 0.8.10
#### 3. Realisation:
* Backup to external hard drive
* Using UNetbootin I created a  bootable Manjaro USB drive. At the moment, Manjaro is not a standard option so you will have to download the right ISO from the Manjaro website. Please make sure to pick the correct version, not only regarding the desktop environment, but taking into consideration your CPU.  For the Lenovo Yoga i choose the 64-Bit Manjaro 0.8.10 KDE version.
![unetbootin](../../Images/installer.jpg)
* Manjaro booted perfectly as a live system. But running the graphical Installer as seen below produced a problem.

The system did not boot to desktop. The problem did obviously did not originate in the primary boot process, as GRUB was working and could be accessed witout a problem.
 After briefly showing the Manjaro boot logo the following "error" stopped 
ManjaroRoot: clean, 129006/28778496 files, 2397472/115106304 blocks
A short research showed, that the issue has been spotted in the wild before. → Manjaro Support Forum
The replies suggested that fstab starts running a hard drive check. Somehow that check failes to complete. Every offered solution to fixing fstab didn't work. Arch Linux Support Forum That meant back to the drawing board. But the way wasn't that far. 

* The next obvious solution was the  "terminal" installer. The following steps can be found at "Installation Guide for Beginners 0.8.2" in an extended version.

After starting the installation, you can chose the installer. The second option is referred to as testing, but offers EFI-Support.
Testing Installer (EFI Support) 
Setting Date and Time →UTC - Europe - Berlin
Assisted Preparation→Erases whole hard drive→Choose your hard drive, In my case sdb→Boot partition size 150mb, as this partition only holds the very basic information, for example the boot loader (GRUB) and the kernel. When writing this, the 3.16 kernel is approximately 75mb big. Even with future increase, this leaves enough space for future updates. Next you will be asked about the swap partition. Even though I use an SSD, I decided to go with a 2 GB Swap partition, to be able to send the system to hybernation. Still, you should decrease the swappiness of the system to reduce SSD wear. That left 80gb for the root partition and  400gb for the home partition. The filesystem was set to ext4 for / and /home.
After confirming your choices you are redirected to the main menu and you can start the installation.
Next comes the configuration of the extracted system and you should set the root password and another user. If necessary, you have to set the locale and keymap.
Finally the bootloader (GRUB) has to be installed.  The hard drive has to be defined. The sample picture shows the installation on sda1. I installed my system on sdb, so the boot manager was installed to sdb1. When asked about the partition table it is vital to use a GPT partition table. Using the GPT instead of msdos/MBR prevented the system from running into the error described above.

After that I was finally able to boot the computer without any problems.
Conclusion: This error could be easily resolved with an additional question in the graphical installer (preferrably with the suggestion to use GPT on UEFI systems).
Bugs after installation:
Setting the backlight does not work. 
The WiFi module does not work.
Multitouch is not enabled


#### 4. Sources:
https://bbs.archlinux.org/viewtopic.php?id=180308&p=1
https://wiki.manjaro.org/index.php/Installation_Guide_for_Beginners_0.8.2
http://bastardadmin.wordpress.com/2014/02/02/manjaro-linux-hochgelobt-doch-leider/
#### 5. Outlook: 
The next guide will show how to enable WiFi in Manjaro for the Lenovo Yoga 13 because the Realtek 8723AU is currently not supported by any kernel. 
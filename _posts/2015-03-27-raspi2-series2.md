---
layout: post
title: RasPi2 Series II - Secure SSH + Static ip + DynDNS
category: blog
published: true
date: '27-03-2015 13:34'
taxonomy:
    category:
        - blog
    tag:
        - ddclient
        - arch
        - linux
        - arm
        - dyndns
        - pam
        - ssh
---
<!--excerpt-->
#### 1. Objective: change SSH settings, change network (static ip), configure DynDNS 
Setup: Lenovo Yoga 13 (2012) Arch
Hardware Requirements: Raspberry Pi 2, MicroSD Card, ssh access
#### 2. Realisation:
* I assume, you have finished the first part of this series or the state of your device resembles it.
* SSH to your RasPi2.
` $ ssh <user>@<RasPi2_ip>`
* Change ssh login method to two-factor authentication
Why? --> Checking your server logs should give you the answer. 
Install FreeOTP / Google Authenticator on your phone (Android/iPhone/Blackberry)
Install libpam-google-authenticator from AUR
` $ yaourt -Sy libpam-google-authenticator`
You will recieve an error when building. 
➟ Edit PKGBUILD , and change line 7 (might be a little different)
 arch=('i686' 'x86_64')
➟ to ➟ 
 arch=('i686' 'x86_64' 'armv7h')
Continue and finish the build / installation process
Edit pam config file
` $ sudo nano /etc/pam.d/sshd`
Add the following line
 auth    required    pam_google_authenticator.so
Edit ssh server config file
` $ sudo nano /etc/ssh/sshd_config`
➟ Add the following line (line 76 for me)
 ChallengeResponseAuthentication yes
Run google authenticator and note your secret key, verification code, emergency scratch code
` $ google-authenticator`
Answer yes (time-based token), answer yes (update /home/<username>/.google_authenticator), answer yes (disallow multiple uses), answer no (increase token validity), answer yes (rate limiting)
Open FreeOTP or Google Authenticator; Scan the QRCode that is provided at the given url, or
Add the key manually:
<username>@<hostname>
Secret ➟ Secret Key
Type ➟ TOTP
Digits ➟ 6
Algorythm ➟ SHA1
Restart sshd service
Login with:
Passwort ➟ <user password>
Verification Code ➟ <dynamically generated key>
* Configure static ip
Find the adapter name: most likely eth0 for the RPi2
Disable the dhcpcd service to prevent the RPi requesting the offered ip.
` $ sytemctl disable dhcpcd@eth0.service`
Make sure, that all other services, that might interfer with our configuration are disabled.
` $ sytemctl disable ifplugd@eth0.service`
` $ sytemctl disable netctl-auto@eth0.service`
` $ sytemctl disable netctl-ifplugd@eth0.service`
I ran into **** problems because I accidently activated several netctl services in a desperate attempt to get this to work.
Create ethernet adapter configuration file²
` $ sudo nano /etc/systemd/network/eth0.network`
Replace eth0 with your adapter name if neccessary
Add the following text to the adapter service file
[match]
Name=eth0
[network]
DNS=8.8.8.8
Address=<youripadress>
Gateway=<yourrouterip>
Further information can be found here².
Reboot and login
* Configure DynDNS with ddclient
I use spdns.de as DynDNS provider, ddclient to register the changed ip with the provider
Install ddclient
` $ sudo pacman -Sy ddclient`
Edit ddclient.conf --> Explanation here
` $ sudo nano /etc/ddclient/ddclient.conf`
#### 3. Sources:
¹https://wiki.archlinux.org/index.php/Google_Authenticator 
²https://wiki.archlinux.org/index.php/systemd-networkd
³https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-two-factor-authentication
⁴http://www.howtogeek.com/121650/how-to-secure-ssh-with-google-authenticators-two-factor-authentication/
⁵http://en.wikipedia.org/wiki/Google_Authenticator
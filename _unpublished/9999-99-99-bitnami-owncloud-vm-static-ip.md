---
layout: post
title: Bitnami Owncloud VM
published: false
---
The Bitnami Owncloud VM is a great way to get your owncloud instance up and running fas, if you have a possibility to host virtual servers.

Some tweak and tipps:

Make that IP static

Change /etc/network/interfaces
before

...
\# The primary network interface
auto eth0
iface eth0 inet dhcp


after
\# The primary network interface
auto eth0
iface eth0 inet static
address \<your ip>
gateway \<your router/gateway>
dns-nameservers \<DNS> \<DNS>

example
auto eth0
iface eth0 inet static
address 192.168.1.111
gateway 192.168.1.1
dns-nameservers 8.8.4.4 8.8.8.8

----------------
Configure trusted domain:
--> Error: You are accessing the server from an untrusted domain.

 /opt/bitnami/apps/owncloud/htdocs/config/config.php:
'trusted_domains' =>
array (
    0 => "FIRST_DOMAIN",
    1 => "SECOND_DOMAIN",
    2 => "THIRD_DOMAIN",
),


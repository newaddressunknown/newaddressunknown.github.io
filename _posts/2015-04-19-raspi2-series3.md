---
layout: post
title: RasPi2 Series III - tmux + weechat
category: blog
published: true
date: '19-04-2015 17:34'
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
---
<!--excerpt-->
#### 1. Objective: 
Use RPi as a webserver to run weechat and configure weechat
Setup: Raspberry Pi 2, ssh
#### 2. Realisation:
* Install tmux and weechat 
`$ sudo pacman -Sy tmux weechat`
tmux vs screen battle fans read on here³
* Create new tmux session
`$ tmux new -s <session_name>`
Start weechat
`$ weechat`
* Configure Weechat
Add and connect to freenode server
`/server add freenode irc.freenode.net`
`/connect freenode`
Change and register your nickname
`/nick <yournickname>`
`/set irc.server_default.nicks "<yournickname>,<yournickname2>"`
`/msg NickServ REGISTER <password> <yourmail>`
You will recieve a confirmation mail with a <personalkey>
`/msg NickServ VERIFY REGISTER <yournick> <personalkey>`
Add and connect to freenode server
`/server add freenode irc.freenode.net`
`/connect freenode`
Join a channel
`/join #archlinux.de`
Autoconnect /-join
`/set irc.server.freenode.autoconnect on`
`/set irc.server.freenode.autojoin "#archlinux.de"`
`/save`
* Further Weechat configuration
`/script`
Select buffers.pl and type i 
Enable ssl 
`/set irc.server.freenode.addresses "chat.freenode.net/7000"`
`/set irc.server.freenode.ssl on`
Auto-authenticate with nickserv
`/set irc.server.freenode.command "/msg NickServ IDENTIFY <password>"`
Please decide for yourself wether it is a good idea to store the password in plain text in the config file.
Save, restart and enjoy
`/save`
`/quit`
* Detach session
To leave tmux and return to terminal while keeping the session alive, press
Ctrl + b then d
* SSH and auto attach tmux
`$ ssh <user>@<hostname> -p <customport> -t tmux attach -t <seesionname>`
#### 3. Sources:
¹http://www.turnkeylinux.org/blog/tmux-screen-alternative
²https://robots.thoughtbot.com/a-tmux-crash-course
³http://www.wikivs.com/wiki/Screen_vs_tmux
⁴https://weechat.org/files/doc/devel/weechat_quickstart.en.html

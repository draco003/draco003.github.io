---
layout: post
title: "OpenSSH Server Security"
date: 2013-03-31 03:30:01 +0200
comments: true
categories: [FreeBSD, Networking, Security]
description: OpenSSH Server Security Hardening Tips on FreeBSD
keywords: openssh,ssh,sshd,freebsd sshd,sshd_config,ssh security 
---
Secure Shell (SSH) is a network protocol that allows data to be exchanged over a secure channel between two computers.

`SSH` is typically used to log into a remote machine and execute commands, it also supports tunneling and we can transfer files using the scp protocol.

Change the default port on which SSH listens for incoming connections.

I'll be using port number 22644 for this example, so I'll add the following lines to the `/etc/rc.conf` file
```
sshd_enable="YES"
sshd_flags="-p 22644"
```

Disable root user login, and use SSH Protocol 2.

Edit and uncomment (as necessary) the following lines in the `/etc/ssh/sshd_config` file
```
Port 22644
Protocol 2
PermitRootLogin no
```

Now we have to restart the OpenSSH server,
``` bash
# /etc/rc.d/sshd restart
```

You might also want to activate SSH Public Key login, check this post.

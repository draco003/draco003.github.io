---
layout: post
title: "Generate SSH Keys"
date: 2013-04-04 06:33:12 +0200
comments: true
categories: [FreeBSD, Security]
description: Generating SSH Keys public private keys pair
keywords: ssh key, freebsd ssh, public ssh key, private ssh key, public key 
---
SSH keys enable you to access a remote machine using ssh protocol using your public/private ssh keys combination.

You can choose not to apply a password, and thus access the machine directly, or you can add more security to the keys and apply a password.

The following command generates a pair of ssh authentication keys for your machine
``` bash
% ssh-keygen -t rsa
```

The default keys will be created under your home directory in the `.ssh` directory, id_rsa and id_rsa.pub

`id_rsa`: This is your private key, keep it safe and never share it with anyone. It should not be readable by anyone.

`id_rsa.pub`: This is your public key, you share it with the server or service you want to authenticate against.

You will also need to apply the following permissions to the keys and the .ssh directory,
``` bash
% chmod 700 ~/.ssh
% chmod 600 ~/.ssh/id_rsa
```

To add your keys to a remote server just copy the `id_rsa.pub` file to that server and add it to the `authorized_keys` file,

also don't forget to fix the permissions.
``` bash
[on remote server]
% cat id_rsa.pub >> ~/.ssh/authorized_keys
% chmod 700 ~/.ssh
```

It's always a good practice to add a password to your keys.

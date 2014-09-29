---
layout: post
title: "Mount ISO File Format"
date: 2013-03-25 04:03:03 +0200
comments: true
categories: FreeBSD
description: HOWTO Mount an ISO cd image into memory disk on FreeBSD
keywords: iso,iso cd image,freebsd,mount iso FreeBSD,mount iso cd 
---
Sometimes I get files in the `iso` format, perhaps ripped of a CD or something and I want to use the files without burning it to a CD.

This is the way I do it on FreeBSD.

First open a terminal, then type the following commands,
``` bash
# mdconfig -a -t vnode -f path/to/file.iso -u 1
# mount -v -t cd9660 /dev/md1 /media/iso
```

Where `path/to/file.iso` is the real path to your file, and `/media/iso` is where you mount the iso.

You must be root to run these commands. This basically attaches a `vnode` type memroy disk from that iso file and creates an md device with the number specified in the `-u` option.

After that I copy the mounted data else where, and then un mount the vnode memory disk created in the above steps.
``` bash
# umount /media/iso
# mdconfig -d -u 1
```

That's all folks!

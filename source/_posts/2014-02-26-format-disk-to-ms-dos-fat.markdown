---
layout: post
title: "Format Disk to MS-DOS FAT"
date: 2013-03-27 06:00:45 +0200
comments: true
categories: FreeBSD
description: HOWTO Format an MS-DOS FAT file system on FreeBSD
keywords: format ms-dos fat,format fat32,freebsd,format ms-dos freebsd 
---
Formatting a USB stick or external drive into `MS-DOS FAT32` file system.

We will be using gpart utility as well as `newfs_msdos`

Follow these steps, run as root.
``` bash
# dd if=/dev/zero of=/dev/da0 bs=1k count=1
# gpart create -s mbr da0
# gpart add -t "\!11" da0
# newfs_msdos -F32 /dev/da0s1
```

Line 1: We will first clear the partition info using `dd` command,
make sure you use the right device name: `/dev/da0` in my case.

Line 2: Create an `MBR` (Master Boot Record) on that device.
Line 3: Add a new mbr partition, we use `!11` type for mbr (with proper escaping for BASH),

when I used the mbr type `gpart` didn't recognize it.

Line 4: Constructs a new MS-DOS FAT file system of type 32.

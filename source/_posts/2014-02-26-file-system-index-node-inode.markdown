---
layout: post
title: "File System Index Node (inode)"
date: 2013-06-30 04:43:29 +0200
comments: true
categories: [FreeBSD, Linux]
description: Display file inode (index-node) under Linux/UNIX [*BSD] systems
keywords: inode, index-node, ls, linux unix inode, unicode file name, arabic file name 
---
The inode is a unique identifier in Linux/UNIX filesystems, it's also known as the index number.

Definition on [nixCraft](http://www.cyberciti.biz/tips/understanding-unixlinux-filesystem-inodes.html "nixCraft inode article"): 
An inode is a data structure on a traditional Unix-style file system such as `UFS` or `ext3`. An inode stores basic information about a regular file, directory, or other file system object.

We can display the inode of a specific file name or a list of files in a directory by using the `ls` command.

Here is a typical output of files inode inside a directory,
``` bash 
$ ls -li
total 13
10376896 -rw-r--r--  1 draco  draco   200 Feb 11 19:01 test.c
10376892 -rw-r--r--  1 draco  draco   480 Feb 11 19:11 test.hex
10376897 -rw-r--r--  1 draco  draco   181 Feb 11 19:10 test
```
The numbers are the inodes of the respictive files.

One of the most common use of inodes is handling filenames with special characters or unicode like UTF-8, you cannot handle such filenames directly from the terminal, so you need to access it using its inode number.

My answer on StackOverflow to a related problem: http://stackoverflow.com/a/16216044/1708778
 
-- Further reading: [Understanding Filesystem Inodes](http://www.onlamp.com/pub/a/bsd/2001/03/07/freebsd_basics.html "Understanding Filesystem Inodes") (*BSD DEVCENTER ONLamp.com*)

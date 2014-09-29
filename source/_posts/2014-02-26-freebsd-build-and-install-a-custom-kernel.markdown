---
layout: post
title: "FreeBSD: Build and Install a Custom Kernel"
date: 2013-07-02 21:33:00 +0200
comments: true
categories: [FreeBSD]
description: Building and Installing a Custom FreeBSD Kernel
keywords: custom kernel, build kernel, compile kernel, build custom modules kernel, freebsd kernel, freebsd IPFW custom kernel
---
FreeBSD is a very powerful OS and you can even achieve higher performance by modifying the Kernel and using the modules you need.

Before playing with the kernel you need to have the full FreeBSD source tree installed. The source tree lives in `/usr/src` 

If you don't have it then you can get it using subversion `svn`, follow this guide: [Section A.5, “Using Subversion”](http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/svn.html "A.5. Using Subversion").

A great resource for building a custom kernel is [Chapter 9](http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/kernelconfig-building.html "Building and Installing a Custom Kernel") of the FreeBSD Handbook, it should be your main guide through the process.

<!-- more -->
First cd to `arch/conf`, where arch is the architecture of your machine -- use `uname -a` to display it, we will assume i386 same as the manual. Afterwards copy the `GENERIC` configuration file to a new folder under `/root/kernels/` name it in capital letters (traditions) we will use `MYKERNEL` here, you could use whatever you prefer. The manual says it's better to name it after the hostname of your machine if you are working with multiple machines.

-- Tip:
When finished customizing the kernel configuration file, save a backup copy to a location outside of `/usr/src`. Do not edit `GENERIC` directly.

Alternately, keep the kernel configuration file elsewhere and create a symbolic link to the file in `i386`.
``` bash
# cd /usr/src/sys/i386/conf
# mkdir /root/kernels
# cp GENERIC /root/kernels/MYKERNEL
# ln -s /root/kernels/MYKERNEL
```
Next start editing your new configuration file `MYKERNEL`. You are advised to check the [Handbook](http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/kernelconfig-config.html "Kernel Configuration") section.

One of the first things to edit is the ident argument, change it to your custom kernel name, in our case `MYKERNEL`.
```
ident MYKERNEL
```
When I first built my custom kernel it was for the purpose of optimizing a server for testing `IPFW` and other firewall functionality, I also removed all the unnecessary jargon and anything I don't need for the purpose of this server.

You need to spend some time with the configuration file options and the kernel config in the handbook to get a better sense of each configuration.

After you are done, save the file and head over to the next step.

-- **Note**: It is required to have the full FreeBSD source tree installed to build the kernel.
Now you are ready to compile your new kernel, using `buildkernel` and specifying your kernel name `MYKERNEL`.

Next install your new kernel using `installkernel` command.

*The above steps are gonna take some hours depending on your hardware config so be prepared, with your favorite book.*

Please make sure you have a recent backup before installing the new kernel.
``` bash
# cd /usr/src
# make buildkernel KERNCONF=MYKERNEL
# make installkernel KERNCONF=MYKERNEL
```
That's it. Now you just need to restart and experience the power!
``` bash
# reboot
```
The new kernel will be copied to `/boot/kernel` as `/boot/kernel/kernel` and the old kernel will be moved to `/boot/kernel.old/kernel`. Now, shutdown the system and reboot into the new kernel. If something goes wrong, refer to the troubleshooting instructions and the section which explains how to recover when the new kernel does not boot.

A great advice by [Rhyous](http://www.rhyous.com/2012/05/09/how-to-build-and-install-a-custom-kernel-on-freebsd/ "Custom Kernel") worth mentioning is to check your disk space before compiling your new kernel.

If you just want to edit one module, in my case I was working on IPFW, you probably don't have to recompile the whole system, in order to compile faster you can edit the /etc/make.conf file

The `MODULES_OVERRIDE` variable will only build the specified modules instead of building everything.
```
MODULES_OVERRIDE = net netinet/ipfw
```
More information is available from the handbook.

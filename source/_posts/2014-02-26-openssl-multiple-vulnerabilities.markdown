---
layout: post
title: "OpenSSL Multiple Vulnerabilities"
date: 2013-04-06 20:00:56 +0200
comments: true
categories: [FreeBSD, Security]
description: FreeBSD OpenSSL multiple vulnerabilities
keywords: freebsd,openssl,openssl vulnerabilities,fix openssl vulnerabilities,FreeBSD Security Advisory,pgp signatures 
---
FreeBSD includes software from the OpenSSL project. A flaw in the OpenSSL handling of OCSP response verification could be exploited,to cause a denial of service attack.

The original topic from Security Advisory can be checked [here](http://www.freebsd.org/security/advisories/FreeBSD-SA-13:03.openssl.asc "Security Advisory Link").

It explains the problem and how to fix the source code of your current installation.

I used the second solution which is "To update your vulnerable system via a source code patch:"
a) Download the relevant patch from the location below, and verify the
detached PGP signature using your PGP utility.
``` bash
[FreeBSD 8.3 and 9.0]
# fetch http://security.FreeBSD.org/patches/SA-13:03/openssl.patch
# fetch http://security.FreeBSD.org/patches/SA-13:03/openssl.patch.asc
# gpg --verify openssl.patch.asc

[FreeBSD 9.1]
# fetch http://security.FreeBSD.org/patches/SA-13:03/openssl-9.1.patch
# fetch http://security.FreeBSD.org/patches/SA-13:03/openssl-9.1.patch.asc
# gpg --verify openssl-9.1.patch.asc
```
I'm using `RELEASE-9.0` at the time of this writing, so I used the first method.

If you don't have `PGP` installed yet, you will have to get `GNUPG` using the following commands,
``` bash
# portmaster security/gnupg


[not using portmaster?]
# cd /usr/ports/security/gnupg
# make install clean
```

You also need to download the Security Officer PGP Key and import it.
``` bash
# fetch http://www.freebsd.org/security/so_public_key.asc
# gpg --import so_public_key.asc
```

Now you have the Security Advisor publick key installed, you can go ahead and verify the downloaded patch using the gpg --verify command,

You made it that far, now apply the patch (in my case it was located in my home directory),
b) Execute the following commands as root:

``` bash
# cd /usr/src
# patch < /home/draco/openssl.patch
```

Then `buildworld` and `installworld` (for more info consult the [Handbook](http://www.freebsd.org/handbook/makeworld.html "Make Wolrd FreeBSD Handbook"))

This will take a while, so go pray or read a book  =)
``` bash
# cd /usr/src
# make buildworld
# make installworld
```
Now you need to restart your machine,
``` bash
# shutdown -r now
```
That's all folks.

P.S Consult the original topic: http://www.freebsd.org/security/advisories/FreeBSD-SA-13:03.openssl.asc

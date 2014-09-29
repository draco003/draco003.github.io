---
layout: post
title: "HTTPS Proxy Test in C"
date: 2013-07-03 20:53:23 +0200
comments: true
categories: [FreeBSD, Linux, Networking]
description: Gist HTTPS Proxy Test in C
keywords: gist https-proxy test gcc FreeBSD https proxy tester 
---
I wanted to share my first contribution to Gist. It's an HTTPS proxy tester using cURL library in C.

-- Forked from [mj](https://gist.github.com/mj/5102778 "Original Gist by mj").
<!-- more -->
<script type="text/javascript" src="https://gist.github.com/draco003/5448626.js"></script>

To compile under FreeBSD using GCC:
``` bash
$ gcc -Wall -L/usr/local/lib/ -I/usr/local/include/ -lcurl https-proxy-test.c -o https-proxy-test
```

Usage: `https-proxy-test <ip:port>`
``` bash
$ ./https-proxy-test 123.123.123.123:8080
```

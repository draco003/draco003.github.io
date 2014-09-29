---
layout: post
title: "Convert BIN/TOC Image to BIN/CUE"
date: 2013-07-26 06:18:41 +0200
comments: true
categories: [FreeBSD]
description: HOWTO Convert bin/toc cd image to bin/cue format on FreeBSD
keywords: bin/toc, bin/cue,cd image,iso/cdr,iso,freebsd,bchunk,bin cue, toc2cue, brasero, toc to iso, cdrdao 
---
So I was ripping a CD using Brasero and the output I got was a binary file along with a TOC file.

I wanted to mount this binary or convert it to ISO, so I came across this utility, toc2cue. which is is part of *cdrdao* port.

First install cdrdao, a read-write utility for CDs in disc-at-once mode
``` bash
# portmaster sysutils/cdrdao
```
Then run the following command
``` bash
# toc2cue filename.toc filename.cue
```
And you would have a cue/bin format which you can convert to ISO later using the procedures in this [article](http://draco003.github.io "BIN/CUE to ISO/CDR Article").

Anyway I could have just used dd to create the ISO from a CD located at /dev/sr0 like so
``` bash
# dd if=/dev/sr0 of=/path/to/file.iso
```
But it was nice knowing these utilities in case I came across these formats in the future.

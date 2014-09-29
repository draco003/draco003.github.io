---
layout: post
title: "Convert BIN/CUE Image to ISO/CDR"
date: 2013-03-25 20:14:17 +0200
comments: true
categories: FreeBSD
description: HOWTO Convert bin/cue cd image to iso/cdr format on FreeBSD
keywords: bin/cue,cd image,iso/cdr,iso,freebsd,bchunk,bin cue 
---
If you are familiar with torrents, you might have downloaded a `bin/cue` cd image before.

The bin/cue CD format is used by some non-Unix cd-writing software, but it's not supported by most other programs. `image.bin` is the raw cd image, and `image.cue` is the track index file containing track types and offsets.

On FreeBSD I'll be using the `bchunk` command. Read more on the man page `bchunk(1)`

You can install bchunk from ports, first update your ports tree and then build it.
``` bash
# portsnap fetch update
# cd /usr/ports/sysutils/bchunk
# make install clean
```

Then just run bchunk.
``` bash
# bchunk -v image.bin image.cue basename
```

The `basename` is used for the beginning part of the created track files.

This will create a new iso and cdr files which can be mounted to a `vnode` memory disk. 

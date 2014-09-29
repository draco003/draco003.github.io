---
layout: post
title: "TOPS: Total Open Station"
date: 2013-03-29 03:58:33 +0200
comments: true
categories: [FreeBSD, Linux, Surveying]
description: TOPS Total Open Station GPL Free Software
keywords: total station,total open station,opensource total station,linux total station,total station data linux,python total station 
---
{% img center /downloads/images/tops.png %}
[Total Open Station](http://tops.iosa.it/ "TOPS Website") (**TOPS** for friends) is a free software program for downloading and processing data from total station devices.

It's a free software licensed under GNU GPLv3, based on Python and it's a cross-platform solution.

Works on FreeBSD, Linux, and even Windoze.

There is also a wide variety of supported devices: *Lecia*, *Nikon*, and *Zeiss*.
<!-- more -->

It's relatively easy to support new devices by connecting them to the TOPS and collect data dumps from the device. Then file a support ticket in the bug tracker on the TOPS website. If you are good with Python you can edit the code and write a module to work with your model.

Check this page for various ways of installation: http://tops.iosa.it/installing.html#installing

I installed it on FreeBSD using `pip`.
``` bash
$ virtualenv tops-environment
$ source tops-environment/bin/activate
(tops-environment)[draco@myBox ~/tops]$ 
$ pip install totalopenstation
$ totalopenstation-gui.py
```

Line 1: change directory to where you want TOPS installed,

and run the `virtualenv` command to create a new virtual environment called `tops-environment`.

Line 2: Execute the `activate` shell script to activate the virtual environment.

Line 3: You should see something like that line, which indicates the activation of virtualenv.

Line 4: Install TOPS using the pip installer.

Line 5: Run the GUI TOPS program.

The next time you want to run the program, follow these steps:
open a terminal
cd to the directory where the virtual environment was created
`source tops-environment/bin/activate` to enter the virtualenv
`totalopenstation-gui.py` will start the program

This is an amazing piece of software in the opensource world, instead of proprietary software that only works on Windoze and are not Free.

Enjoy Your Surveys!

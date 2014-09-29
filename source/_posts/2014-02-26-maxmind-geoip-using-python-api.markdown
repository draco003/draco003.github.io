---
layout: post
title: "Maxmind GeoIP using Python API"
date: 2013-07-27 10:10:48 +0200
comments: true
sharing: true
categories: [FreeBSD, Linux]
description: Pure Python API integration with Maxmind GeoIP Databases
keywords: pygeoip, python, maxmind, GeoIP, freebsd, linux, geolocation 
---
GeoIP is a very important addition to projects, moreover keeping the GeoIP databases up-to-date. I'll be using pygeoip python API along with Maxmind binary databases to create a simple python script that returns country name according to the supplied IP address.
<!-- more -->
First of all to maintain an up-to-date version of the GeoIP databases from [Maxmind](http://dev.maxmind.com/geoip/legacy/install/country/ "Maxmind Installation"), I wrote the following BASH script (maxmind.sh)
``` bash
#!/bin/sh
GeoIP_DIR="/home/draco/GeoIP"
echo $(date +%Y%m%d)
wget -cN --limit-rate=30k -P $GeoIP_DIR  http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz
wget -cN --limit-rate=30k -P $GeoIP_DIR  http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
wget -cN --limit-rate=30k -P $GeoIP_DIR  http://download.maxmind.com/download/geoip/database/asnum/GeoIPASNum.dat.gz
gunzip -k $GeoIP_DIR/*.gz
mv -f $GeoIP_DIR/*.dat /usr/local/share/GeoIP/
echo "DONE"
```
You could omit the GeoLiteCity and GeoIPASNum if not interested. This shell script will check the Maxmind website for a new version of the currently downloaded databases if any, then it extracts it into /usr/local/share/GeoIP/ to be accessed from the python script.

It's propably best to create a crontab job to check updates [every week](http://superuser.com/a/67353 "SuperUser | Weekyly Cron") or even daily, according to your requirements.
``` bash
$ crontab -e
```
Then add the cronjob, for example to run it every week on Sundays I use the following command,

--make sure you have created `/var/log/GeoIP.log`
``` bash
0 0 * * 0 /home/draco/GeoIP/maxmind.sh >> /var/log/GeoIP.log
```
Now to the python script (geoip.py), which is based on [pygeoip](https://github.com/appliedsec/pygeoip "pygeoip on GitHub"), it takes one argument "IP" and returns full version name of country + ISO shortcode.
``` python
#coding: utf-8
#!/usr/local/bin/python

import sys
import pygeoip

gi4 = pygeoip.GeoIP('/usr/local/share/GeoIP/GeoIP.dat', pygeoip.MEMORY_CACHE)
ip = sys.argv[1]
code = gi4.country_code_by_addr(ip)
name = gi4.country_name_by_addr(ip)

print code + ' -- ' + name
```
Usage as follows,
``` bash
$ python geoip.py 98.139.183.24
```
That's pretty much it.

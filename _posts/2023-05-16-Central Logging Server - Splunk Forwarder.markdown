---
layout: post
title: "Central Logging Server - Splunk Forwarder"
description: "I went out on the search for a solution to my over-engineering problem and found that I had options (people love options)."
tags: [Central Logging, Splunk Forwarder, rSyslog, Logrotate]
# thumbnail image for post
feature_image: "images/2023-05-16/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2023-05-16
imagetag: "images/2023-05-16"
# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
#meta_description: ""

# optional
# if you enabled image_viewer_posts you don't need to enable this. This is only if image_viewer_posts = false
#image_viewer_on: true
# if you enabled image_lazy_loader_posts you don't need to enable this. This is only if image_lazy_loader_posts = false
#image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false
---


Continuing on with rebuilding the homelab, since I have two boxes set up (one main "server" and one "router/server/gateway/firewall" combo), I found that there was a real need to have separate logging on this box to be forwarded to my [Splunk](https://www.clouddefenselabs.com/posts/2022-07-03-Splunk%20Enterprise%20Pt.%201%20(Installation)) instance. Whenever I took my main server down which had my Splunk instance on it,<!--more--> anything that was sending syslogs to it via the udp port (and not using a universal forwarder, i.e. Routers, Switches, etc) would have that data lost. So any data that my firewall/router was sending my Splunk instance was essentially gone. While in a homelab environment this isn't critical (to most), in production this would not be best practice. Plus, why wouldn't I want to over-engineer something?

![Syslog VM]({{page.imagetag}}/1.png){:data-align="center"}

### rSyslog to the rescue
I went out on the search for a solution to my over-engineering problem and found that I had options (people love options). There are several solutions, I heavily thought about using [Graylog](https://www.graylog.org/), and installing it on my lowly Optiplex 3020 but the amount of resources it required to set up and utilize left me wondering if I really needed something that heavy. Then there was [Syslog-ng](https://www.syslog-ng.com/) which I'll admit, I almost took a stab at. But then it occurred to me, most unix systems have [rsyslog](https://www.rsyslog.com/) built in, why not use that to collect data and forward to Splunk? It was time to fire up a Diet-pi image on Proxmox (very low resource usage), mount a USB drive for log storage to the VM, and start collecting logs from my firewall, DNS server and other micro servers that may reside on this box.


![Syslog VM Resources Setup]({{page.imagetag}}/2.png){:data-align="center"}

#### The Method
So after setting up the Diet-pi VM (8 GB OS Disk, 50GB USB Disk Image (Raw qcow2 - Mounted at /mnt/logs), 1 CPU/512MB ram) I went into the Diet-pi configuration and installed appropriate software:

```bash
$dietpi-software
```
![Dietpi Software Selection]({{page.imagetag}}/3.png){:data-align="center"}

Select the following software - rSyslog and Logrotate.

![rSyslog and Logrotate selection]({{page.imagetag}}/4.png){:data-align="center"}

Then select "Install" on the next screen then click OK, the installer will continue to install the software and you will be presented back to the bash shell. Now lets set up our Splunk user.

```bash
$adduser splunk
$usermod -aG splunk splunk
```

This command will add the user Splunk, and assign him to the group "splunk". We need this user in order to properly assign the logs to the correct user when the Splunk Forwarder is installed (so it's not sent off as Root).

Now it's time to set up rSyslog.

```bash
$nano /etc/rsyslog.conf
```
In here we need to add some settings in, first we need to enable the UDP and TCP modules under each section. Uncomment the following fields:

```bash
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")
```
![rSyslog conf settings - UDP and TCP Inputs]({{page.imagetag}}/5.png){:data-align="center"}


Then we need to add our rules for accepting inputs for our remote devices:

```bash
#Rules for Processing Remote Logs
$template RemoteLogs,"/mnt/logs/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& ~
```
(This assumes that you have already mounted your /mnt/logs folder with a USB drive, if you have not please do so or expand your storage from 8gb to a bigger drive as 8gb will fill up fast)
![rSyslog conf settings - Rules]({{page.imagetag}}/6.png){:data-align="center"}

Now we restart/enable rsyslog:

```bash
$ systemctl restart rsyslog
$ systemctl enable rsyslog
```

Now lets check out our /mnt/logs folder:

![Folders for log files]({{page.imagetag}}/7.png){:data-align="center"}

New folder is good, 10.50.2.1 is a firewall IP address, thats a good thing. Dietpi is the hostname for the internal logs. Checking one of the log files:

![Looks like data to me]({{page.imagetag}}/8.png){:data-align="center"}

Now lets set up LogRotate so that we don't have logs just building up endlessly. While the best practice for long term use may be setting up Hot, Warm and Cold archival storage (especially for businesses or public sectors who follow any sort of Cybersecurity Frameworks requiring log retention), I'm going to set this up to dump log files after a certain timeframe, as I only need to possibly review logs at this particular gateway at most for a few weeks. It's going to be sent off promptly to another server (i.e. Splunk) for better review/archival if needed.


### LogRotate
First lets make a backup of our current LogRotate config for rSyslog:

```bash
$cp /etc/logrotate.d/rsyslog /etc/logrotate.d/rsyslog
```

then

```bash
nano /etc/logrotate.d/rsyslog
```
I am going to remove everything in the brackets and add:

```bash
rotate 14 
maxage 14
missingok
compress
notifempty
```
What this does:

rotate 14 - 14 log files to keep
maxage 14 - Rotate logs every 14 days
missingok - If its missing - Continue the rotation and suppress the error message
compress - Compress the log file (gzip) - Helpful for size + splunk forwarder
notifempty - Do not rotate log file if its empty

![Logrotate Configuration]({{page.imagetag}}/9.png){:data-align="center"}

### Splunk Forwarder setup

This is the easiest part - We can refer back to the post I made [here](https://www.clouddefenselabs.com/posts/2022-07-05-Splunk%20Enterprise%20Pt.%203%20(Forwarders%20on%20Ubuntu%20and%20Dashboards-Applications)%20). Once we set this up, our deployment server (otherwise known here in the homelab as the main server as I don't have a separate Deployment Server) will instruct the Splunk Forwarder on this instance where to collect log files. This provides the TCP protocol of forwarding our log data, and essentially creating a "Centralized Syslog" server for our Splunk instance.
---
layout: post
title: "Homelab Tools and Resources - Starter kit"
description: "In this post I wanted to list out my top five softwares and resources I use in my homelab (that I really should use more often) when it comes to homelabbing"
tags: [cybersecurity, tools, homelab]
# thumbnail image for post
feature_image: "images/2022-07-14/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-07-14
imagetag: "images/2022-07-14"
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


Managing your #homelab can be simple, however some tools can make managing your day-to-day adventure MUCH simpler, with less headache and hassle, depending on your use case. For example, for those starting out (if you were like me) you may have been accustomed to Windows and don’t know a lot of Linux command-line, so something that utilizes a GUI (graphical user interface) is better suited for your needs.
<!--more-->
In this post I wanted to list out my top five softwares and resources I use in my homelab (that I really should use more often) when it comes to homelabbing.

1. [GitHub – awesome-selfhosted/awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)
Awesome Selfhosted GitHub is a list of software’s and libraries every homelabber should know about. This list provides everything a homelabber would want, categorized neatly, and kept up with by the masses. From automation to media servers to hypervisors, this list has it all.

2. [Visual Studio Code](https://code.visualstudio.com/)

Visual Studio Code is available for all operation systems and allows us to edit text files easily from the computer we are working on. Why is this important when we can just SSH into the box and open nano, vim, vi, or GNU emacs? Because Visual Studio Code allows this as well! Best of all you can also view the folder structure within and save the file to whichever folder you need to on the remote server, how cool is that?

![Visual Studio Code - Dark Mode]({{page.imagetag}}/2.png){:data-align="center"}

3. [PuTTY – a free SSH and telnet client for Windows (or Windows Terminal)](https://putty.org/)

Putty and Windows Terminal are useful tools when it comes to using SSH from Windows. Not terribly much to be stated here. If you use a linux based desktop already when you are in luck because you have terminal (usually) built in.

4. [Proxmox](https://www.proxmox.com/en/) or [ESXI | VMware](https://www.vmware.com/products/esxi-and-esx.html)

While I’m not here to debate on whether Proxmox or ESXi (or any other Hypervisor – Hyper-V, XCP-NG, Xen, etc), the biggest point is, is that a Hypervisor is one of your biggest tools you can use and should be using to make your homelab that much better. If you want to take your homelab game further, look into [Docker](https://www.docker.com/) as well, and venture down the Kubernetes path!

5. [Plex Media Server for Windows, Mac, Linux, FreeBSD and More](https://www.plex.tv/media-server-downloads/)

Most homelabbers I see start here, self-hosting their own Media Server, and for good reasons. The pile of DVD’s stacking up, not wanting to constantly change DVD’s, but yet not wanting to pay to stream it. The ol’ catch 22 of first world problems.

So, there you have it, my top five tools and resources that got me started into the deep end of homelabbing, what most people started off with, and a good starting point to get into this addiction learning experience.




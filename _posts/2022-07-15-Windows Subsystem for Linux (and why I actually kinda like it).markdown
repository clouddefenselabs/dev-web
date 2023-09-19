---
layout: post
title: "Windows Subsystem for Linux (and why I actually kinda like it)"
description: "This post is more of just some high points of why I like Windows Subsystem for Linux"
tags: [cybersecurity, Vulnerability Scanner]
# thumbnail image for post
feature_image: "images/2022-07-15/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-07-15
imagetag: "images/2022-07-15"
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


This post is more of just some high points of why I like Windows Subsystem for Linux. I’m probably going to get flak about why I should pick one or the other (probably more so why I should stick strictly to Linux based OS’s for my daily driver) but when it comes to production and what works with most systems, I find the happy medium to be Windows and using WSL2.
<!--more-->
For starters, WSL2 gives you a command line experience where you can freely SSH into your servers in your homelab without having to download Putty or a third-party tool on Windows. This makes it so much nicer that you can open up Windows Terminal, have it set directly to use the Ubuntu, Debian, or whatever flavor/distro you desire/installed, and go immediately to work:
![Windows Subsystem for Linux - Terminal]({{page.imagetag}}/2.png){:data-align="center"}

Second, instead of having a dedicated VM for Ansible, Terraform, or any other platform you need/use for automation on your server, you can have your “control node” on your DESKTOP! This way you can freely bring up and tear down your servers without having to constantly build up your “control node”. Couple this with hosting your code (sans password files) on GitHub (or your own self-hosted Git repository) and you are set to push/pull code as you see fit, all from the comfort of your laptop/desktop.

Third, Docker still works in WSL2. If you ever wanted to run Docker without having to emulate it, you can do that now. This in and of itself is cool, and you have better performance from your Docker containers.

Forth, no more dual booting. Have Windows only programs and don’t want to use WINE? Or better yet, you play games only suited for Windows and hate having to dual boot? How about needing Outlook (without using the clunky Outlook for Web), or Adobe products for work and don’t use Apple products? WSL2 to the rescue!

Finally, hardware support. While this has become less of an issue today, for some hardware (specifically laptops) it’s still prevalent (looking at you Nvidia GPU and various WiFi drivers). Installing these drivers initially can be overwhelming for someone who is new to the Linux world and using WSL2 can help them get a taste of Linux while being completely hardware compatible.

A bonus I will add is that when you are using Visual Studio Code to write out your automation, or edit any of your files for your homelab, the integration with WSL2 is absolutely seamless and well worth it. While VS Code works on all of the major OS’s, having this integration with WSL2 is a complete win in my book.

![VS Code installed and integrated with WSL2]({{page.imagetag}}/3.png){:data-align="center"}

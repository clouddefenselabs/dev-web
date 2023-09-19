---
layout: post
title: "Deeper Dive into Nessus Scanning"
description: "I wanted to point out some common issues I’ve seen that may be overlooked when people set up their Nessus Scanners and may not know what to look for."
tags: [cybersecurity, nessus, vuln scanner, homelab]
# thumbnail image for post
feature_image: "images/2022-07-07/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-07-07
imagetag: "images/2022-07-07"
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

So, after a few days of setting up my Nessus (Essentials) Scanner for my network and setting up perfect scenarios for people to look out for I wanted to share some things I’ve seen both in the workplace as well as in my homelab. I have my Nessus Scanner set up to scan most of my important hosts, servers, firewall/router, etc., anything I deem important due to 
<!--more-->
licensing constraints (max of 16 IPs scanned). I wanted to point out some common issues I’ve seen that may be overlooked when people set up their Nessus Scanners and may not know what to look for.

Below is a screenshot of two Linux Servers in my environment that I have scanned, one being a RHEL 8 (splunk.xxxx.xxx) VM and the other being an Ubuntu 22.04 VM (edgevm.xxxx.xxx):

![6 Vulnerabilities in one, none in the other?]({{page.imagetag}}/2.png){:data-align="center"}

From initial look, it would appear that the RHEL VM has 6 Vulnerabilities (true) and the Ubuntu VM has none (Yay!), however it is important to dive deeper into the scans, as any good Cybersecurity Analyst would do. We can see in the following photo a neat little Authentication Failure Plugin - [117885](https://community.tenable.com/s/article/Understanding-Plugin-117885-identifying-Intermittent-Failure-in-Scan-Results) 

![Intermittent Authentication Failure – Plugin # 117885]({{page.imagetag}}/3.png){:data-align="center"}
What does this mean and why did this happen? Let’s click in further:

![Rate Limiting SSH?]({{page.imagetag}}/4.png){:data-align="center"}

Something is Rate Limiting my SSH commands into this VM. This tells me one of two things (from prior experience) – Network Issues (very prevalent when the Nessus Scanner is remote, such as when scanning into AWS Cloud VM’s), Network Issues in very noisy/busy networks (such as major infrastructures), or something on the VM/Host slowing down SSH traffic (this is likely the culprit). I need to check my logs:

![FYI, I set this up as a test for Fail2Ban, but misconfiguration can haunt you and be a learning experience]({{page.imagetag}}/5.png){:data-align="center"}

Fail2Ban (noted here in this fail2ban [blog](https://www.clouddefenselabs.com/posts/2022-07-06-Fail%20to%20Ban%20(You%20Need%20It))) was the culprit in blocking my access. Double checking Nessus Plugin [19506](https://www.tenable.com/plugins/nessus/19506) (Nessus Scan Information) tells us that we weren’t getting Credentialed Scans either (another factor that you aren’t getting good scans)

![Credentialed checks : no]({{page.imagetag}}/6.png){:data-align="center"}

After fixing fail2ban (reconfiguring it to not ban my subnet IP’s... for now) I reran the scan again. This time the results look more promising (Still no Vulnerabilities noted – I actually keep this server patched fairly well/up to date.):

![Still no Vulnerabilities noted – I actually keep this server patched fairly well/up to date.]({{page.imagetag}}/7.png){:data-align="center"}

Valid Credentials and No Issues Found as noted above. Plugin 19506 (Nessus Scan Information) also shows a better story:

![Nessus Plugin 19506]({{page.imagetag}}/8.png){:data-align="center"}

There are a number of other Informational Plugins (Info Plugins, I’ve heard some call “Cat 4” or Category 4) plugins as well that are useful to look at and dig through, and some show up depending on what OS/Device you scan. If you want to look at what Software you have on your system? Look for the Software Enumeration Plugin : 22869 (SSH) or 20811 (Windows). The OS Identification? There’s a plugin for that. If Log4j is installed? Yep there’s a plugin. Nessus/Tenable has defined several plugins that you can find [here](https://www.tenable.com/plugins), and if you type in the plugin number or name of what you are looking for, it will give you not only the vulnerability, but also the Risk Information (Risk Factors, CVSS Scores, Severity, etc):


![Nessus Plugin 162735 - Ubuntu Security Update]({{page.imagetag}}/9.png){:data-align="center"}
---
layout: post
title: "Upgrading Homelab Hardware - Revamping and going back to VMware"
description: "Upgrade on hardware to downgrade the powerbill"
tags: [VMWare, ESXI, Cluster, homelab]
# thumbnail image for post
feature_image: "images/2022-09-03/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-09-03
imagetag: "images/2022-09-03"
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

So after spending the past few weeks of tinkering with Proxmox/KVM, diving into Kubernetes/K3S (building, destroying, and doing that probably 20 or so times), using Terraform and Ansible to Build, destroy and repeat, I finally realized that I needed to come back to basics and get back to the enterprise(y) world of software, vulnerability management, and the cloud. <!--more-->That’s not to say that Kubernetes and K3S/K8S isn’t enterprise or the future, but for the homelab currently, I need to redraw my steps and incorporate basics first before introducing K3S/K8S back into it.

So for now, back to VMware’s world I go, but first I need to upgrade some hardware:

### Hardware Upgrades

![HP 800 G3]({{page.imagetag}}/1.png){:data-align="center"}

I had three of the HP 800 G3’s – Each one of these HP 800 G3’s came with an I5 7500 (4C/4T) a 250 GB SATA SSD and 8 GB’s of RAM. When I ordered these units I originally purchased an additional 8 GB’s of RAM and had some random NVMe Drives – a 1 TB NVMe and a 128GB NVMe. I also had 2x PCIe x4 NVMe adapters to fit.

The HP 800 G3’s can hold an NVMe on the motherboard, so to hold an additional NVMe drive, I would have to put one of the additional NVMe’s on an adapter. I ordered another adapter for a total of three and an additional two (2) 128 GB NVMe SSD’s. These are going to serve as Boot Drives for the ESXI Hypervisors. Unfortunately with the 7.03 release of ESXI, booting from a USB drive can cause problems if the scratch partition isn’t stored elsewhere, and I don’t feel like fooling with a separate scratch partition. Plus if I have to reboot, NVMe drives boot slightly quicker than USB.

I also needed more RAM, as I wanted to run vCenter to manage the newly designed three (3) node cluster (which will be described in the future posts). vCenter unfortunately can not have a portion of it offloaded onto a separate Windows OS (I was hoping to put this on my workstation) like you could with 6.7, therefore it will eat a portion of my RAM (default 12 GB of ram, however in a future post I’ll show you how you can get by with 6 GB of ram (It will scream at you…):

![6 GB of Ram – It hates it but it works!]({{page.imagetag}}/2.png){:data-align="center"}

So, since I need more RAM, and these HP 800 G3’s can take up to 64 GB of ram each, why not order more. Two additional sticks of 16 GB of ram for each machine should suffice (32 GB + 16 GB original = 48 GB x 3 Nodes = 144 Gigawatts Gigabytes!). That should be plenty of room for VM’s, and is more RAM than I had in my R420 with 120 GB of RAM. I will say that my CPU Core Count is a lot lower (40 Cores vs 12 Cores) but the processor is MUCH newer and the core clock is much more improved. Plus you can over-provision CPU in VM’s as CPU clocks aren’t all being used at once.

I’m still only using a single GB NIC for the time being. After countless hours of research on Reddit, Stackoverflow, VMwares KB’s, etc., I have realized that in a homelab/testing environment, combining the VMotion NIC with the Management NIC is perfectly fine (IN A HOMELAB/TESTING ENVIRONMENT). In the future I may upgrade to 2.5GB/10GB but I also need to upgrade the network infrastructure as well. At this time I don’t see a need for it.

So overall each node looks like this:

- I5 7500 – 4C/4T
- 48 GB RAM
- 128GB NVMe on PCIe x4 (Boot for ESXI)
- 256GB SATA SSD (Datastore)

and the primary node (for the time being):
- 1 TB NVMe on Motherboard

I plan on upgrading the remaining two with a 1TB NVMe on the motherboard in the future when I need more room.

Each node will also be connected back to my NAS for additional storage via NFS (mainly for ISO Storage/Backend Storage for logs – Splunk and Security Onion).

### Future Posts (Upcoming Days)

I am going to go over setting up ESXI on each host, and setting up the initial SSL Certificate (a wildcard certificate that I borrowed stole from my pfSense router). Even though this is a #homelab, and I am not exposing these servers to the worldwide web (ever), I always want to follow best practices with using a SSL Certificate from a domain I own, plus it keeps Nessus off my back when I start scanning (Looking at you [51192](https://www.tenable.com/plugins/nessus/51192) )

Then I’ll go over setting up vCenter and how I set it up for using only 6GB of ram to save on my precious:

![MY PRECIOUS RAM!]({{page.imagetag}}/3.png){:data-align="center"}

From there I will go over diagraming the homelab using tools such as draw.io (apps.diagrams.net now) to start pre-planning the lab/test environment for Active Directory, Vuln Scanner/Log Aggregation, and building out hopefully with cloud integration, etc.

Stay Tuned!
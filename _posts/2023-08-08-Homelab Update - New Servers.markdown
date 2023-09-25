---
layout: post
title: "Homelab Update - New Servers!"
description: "Over the past few months I've had a lot of different things happen, more namely moving different houses. Now I have a dedicated basement for the homelab (and coincidentally.... my office) and I also expanded a bit."
tags: [Homelab, New, Servers, Logs]
# thumbnail image for post
feature_image: "images/2023-08-08/feat-1.jpg"
# disable comments on this page
comments_disable: true
# publish date
date: 2023-08-08
imagetag: "images/2023-08-08"
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


Over the past few months I've had a lot of different things happen, more namely moving different houses. Now I have a dedicated basement for the homelab (and coincidentally.... my office) and I also expanded a bit.<!--more-->

![New Homelab, who dis?]({{page.imagetag}}/1.jpg){:data-align="center"}

#### Whats in your lab... now...
Starting at the top left of the photo is the old NAS, however I <s>borrowed</s> stole some of the RAM sticks from it and bought some from Amazon in order to max out the boxes below. The boxes in the center are Dell Optiplex Small Form Factor 5050's, each with the I7-7700 and 64GB of DDR4 RAM (4x 16GB). All of these nodes are runing ESXI 8. The NAS box above has been upgraded to ESXI 8 as well, and is currently hosting OpenMediaVault as well as being responsible for vCenter Server.

The Lenovo Thinkcenter is a M710Q and serves as my OPNSense box. It comes with an I3-7300 and 8GB of DDR-4 RAM, as well as 128GB SSD for storage/OS. I added an additional NIC via the A+E Key'd PCIE slot and used the VGA port in the back (removed the VGA port and installed the ethernet port).

Vcenter Server Specs:
![VCenter Server]({{page.imagetag}}/2.png){:data-align="center"}
Node Specs
![Each node]({{page.imagetag}}/3.png){:data-align="center"}


To top all of this off, each node has a 1TB NVMe drive (Datastore), 128GB NVMe (boot and 'some' VM's if needed) and a 500GB HDD (except Node 1, which contains a 1TB HDD). 

The plan is to run these in a cluster with separate resource pools, one resource pool for my cloud mirror (mirrors whats in my Oracle Cloud Instance), one resource pool for a test environment (to test work stuff) and one environment for my homelab (home-prod?). All of these resource pools are going to be shared across the nodes, however High Availability isn't going to be implemented as of yet due to the lack of a SAN (Storage Area Network).

In the next post, I am going to start from the ground up (again) this time collecting logs from OPNSense, to a central Syslog Server, to my Splunk Instance, in preparation for the Splunk Certificates I am working on.
---
layout: post
title: "Splunk Enterprise Pt. 1 (Installation)"
description: "Splunk is best known for monitoring, searching through, analyzing, and providing visuals for real time data."
tags: [cybersecurity, Splunk]
# thumbnail image for post
feature_image: "images/2022-07-03/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-07-03
imagetag: "images/2022-07-03"

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

In todays blog post I’m going to go over one of the many SIEM tools widely used today in many enterprises (both in the commercial sector as well as the DOD), [Splunk](https://splunk.com/).

Splunk is best known for monitoring, searching through, analyzing, and providing visuals for real time data. In laymens terms, we can generate pretty, meaningful graphs and charts, from data that is otherwise just lines of log files or computer-generated data (and get notifications from, etc).

So to start, I am going to install Splunk on its own dedicated RHEL 8 virtual machine:

<!--more-->

![ESXI Setup Screen]({{page.imagetag}}/2.png){:data-align="center"}

Now why I choose RHEL 8 instead of Ubuntu or Debian? Quite honestly I wanted to stick to as close to DOD used Operating Systems for my current role (which, if you are currently working in your Cybersecurity/IT role, your homelab should closely mimic technologies you are working with/exposed to as well!). I also have a RHEL Developer License that has several RHEL keys I’ve been meaning to use up.

Splunk has a [Reference Hardware](https://docs.splunk.com/Documentation/Splunk/latest/Capacity/Referencehardware) page that outlines “recommended” hardware they have tested for various environments, however for a HOMELAB, realistically we aren’t going to be able to spare this many cores (yet… One day I will get to this point), so for now I’m going to give this VM 4 cores and 12GB of ram. Also I will add that in a normal “Enterprise” environment, you would have a Search Head or two, a few Indexers, along with your forwarders, however in this environment I will have everything combined into one machine as I do not have a massive environment to collect log files from (yet). Also I’ve already signed up for a Splunk Developer License which gave me 6 months to test (with the option to renew within 10 days of the license expiring) plus bumps my log aggregation from 500MB to 10GB)

![ESXI VM Creation Screen]({{page.imagetag}}/3.png){:data-align="center"}

Then I’ll start the VM up and install RHEL as required, and install my RHEL License:

![Normally I would select “Server without GUI”, however, I plan on accessing Splunk only on my RHEL Machine after I install it (remoting in from ESXi), so I am installing a GUI to have access to it.]({{page.imagetag}}/4.png){:data-align="center"}

Once all set up, I went to Splunks website and downloaded the appropriate RPM Package

![Snippet from Splunks Website July 2022]({{page.imagetag}}/5.png){:data-align="center"}

Then off to the Terminal to install Splunk

![Terminal Installation]({{page.imagetag}}/6.png){:data-align="center"}

![Terminal Installation]({{page.imagetag}}/7.png){:data-align="center"}

Installation instructions can also be found [here](https://docs.splunk.com/Documentation/SplunkCloud/latest/SearchTutorial/StartSplunk) (for starting the web server)

And if all goes well, you should be able to go to http://localhost:8000

![Splunk Login Screen - Log in using the credentials you set up during the install.]({{page.imagetag}}/8.png){:data-align="center"}

Part two will be posted tomorrow where I continue setting up Splunk Enterprise
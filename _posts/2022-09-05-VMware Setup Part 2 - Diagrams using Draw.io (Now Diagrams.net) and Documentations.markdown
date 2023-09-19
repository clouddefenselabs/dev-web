---
layout: post
title: "VMware Setup Part 2 - Diagrams using Draw.io (Now Diagrams.net) and Documentations"
description: "One important part of setting up a #homelab, or any architecture/network infrastructure, is properly diagramming, is figuring out what direction you want to go, and planning out what you want to do with it."
tags: [VMWare, ESXI, Cluster, homelab]
# thumbnail image for post
feature_image: "images/2022-09-05/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-09-05
imagetag: "images/2022-09-05"
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


One important part of setting up a #homelab, or any architecture/network infrastructure, is properly diagramming, is figuring out what direction you want to go, and planning out what you want to do with it.<!--more--> Documenting as you go is important (one aspect of blogging for me is it helps reinforce what I learn), so setting up a Wiki, writing it down on a notepad, using OneNote, etc., and documenting the network diagrams (and how the data will flow throughout your network), is very important is keeping everything in line and keeping everything in focus.

The easiest and most efficient way to start documenting your homelab/network is to draw it out. You can either do this on paper, or by using [Diagrams.net](http://www.diagrams.net/)

### Diagram

![Diagram]({{page.imagetag}}/2.png){:data-align="center"}

Drawing out your homelab in this way serves multiple purposes, for starters it gives you a clear understanding of how your network is laid out and where you can make improvements. Also (and this has been brought up in the past, at least in my experience), if someone (hint hint, a job recruiter, potential future employer) asks that important question of “Can you describe your home network”, you won’t stammer through it or state “Well I know I have a modem that my ISP gave me, a wireless router, and my computer”. NO! Tell that future employer all the cool things you have in your network.

So how is my home network laid out and how am I planning out my ESXI homelab? Here’s a rough draft of each (Note: It’s not perfect, I have to set up VLAN’s still, I don’t have enough managed switches to do so unfortunately, however that it in the works):

*UPDATE 09/15/2022* - I do have vLAN's now and several network and firewall rules set up for my network as I finally figure out a way to use my measily 8 port switch in a meaningful way coupled with my wireless router (had to physically move it). It took some careful thought but I did manage to get 5 separate networks, one for trusted, one for work, one for the homelab, one for guest (only has 10 usuable IP addresses), and one for IOT/Media devices. Work, Guest and IOT are segregated off to only speak to the internet, with the exception of IOT being able to speak to the media server box ONLY and the internet. Homelab is allowed to only the internet and media device as well, but my trusted network is allowed in (only diff). Trusted network has full reign but can not talk to those in Guest, Work, or IOT (for now).

![Home Network Diagram (Draft)]({{page.imagetag}}/3.png){:data-align="center"}

Now what I don’t have listed in this diagram is my private IP ranges, and I plan on doing that in the near future, however I am still planning/documenting everything. What I do want to provide is some amazing Diagrams I did find and get some inspiration from over at r/homelab (or you can simply search “Diagrams” in that subreddit – or go here). These diagrams I believe are above and beyond and perfectly document their homelab, and even do so much better than some “professional” diagrams I’ve seen in the field:

![Diagram]({{page.imagetag}}/4.png){:data-align="center"}
![Diagram]({{page.imagetag}}/5.png){:data-align="center"}
![Diagram]({{page.imagetag}}/6.png){:data-align="center"}

### Documentation

Documenting your homelab or any network, hardware, software, any operation, anything technical is absolutely crucial in this field. There are many purposes for taking good notes, it helps you remember what you did (or didn’t) do either yesterday or 6 months ago, and it helps others understand what you have done in the event that you aren’t there. As I tell my employers, I want to ensure that if I hit the lottery, or get hit by a bus, that whoever takes my place can do what I do. Good documentation is always best practice, even if you “inherit” a mess at your current workplace, you never want to be remembered at a previous employer as “that guy” who has horrible documentation skills. So go ahead and start now, and practice documenting your homelab if you aren’t already. Some tools I’ve used (all are free) to document and write notes are (in no particular order):

- Microsoft One Note
- [DocuWiki](https://www.dokuwiki.org/dokuwiki#)
- [MediaWiki](https://www.mediawiki.org/wiki/MediaWiki)
- [Bookstack](https://www.bookstackapp.com/)
- [Github](https://github.com/) (or Gitlab – Just privatize your repos in either)
- [Wiki.js](https://js.wiki/)

Now what all should you document in your homelab/network? That’s the simple part. Literally everything! I usually break out with various categorized list (either breakout new “books” or “pages” in OneNote or the Wiki’s), however you can do it however you see fit. The following is just an example:

- Document all Hardware
    - Make and Model
    - Serial Numbers of Hardware
    - What is inside
        - Hard Drives
            - Capacities / Serial Numbers
        - Network Cards
        - Other Peripherals
- Document the Network
    - Physical Location of Hardware
    - Starting with Modem/demarc point, list out network topology to endpoints
    - List out any VLANS and their purpose
    - List out Subnets
    - Wifi Networks and Passwords (Encrypt document)
- Document Virtual Machines
    - What hardware are they on
    - What VLANS/Network/Subnets
    - What ports/protocols do they use
    - Are they external facing?
    - What storage do they interact with?
- Document your Backup Plan/strategy
    - You have a backup plan right?
    - Where are backups stored?
    - How are they ran? Nightly?
    - How do you recover from a disaster?
- Document your Future Plans
    - You have future plans right?

Overall there isn’t necessarily a wrong or right way to document or diagram a homelab or any network, but if you don’t do it you are only setting yourself up for failure. While this portion wasn’t necessarily focused just on VMware’s ESXI/Vcenter, it is important to point out that planning and documenting is one step that should be taken into account, even if its just for practice/labbing.
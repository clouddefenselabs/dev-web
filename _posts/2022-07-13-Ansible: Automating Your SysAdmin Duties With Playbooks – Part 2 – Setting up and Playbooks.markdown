---
layout: post
title: "Ansible: Automating Your SysAdmin Duties With Playbooks – Part 2 – Setting up and Playbooks"
description: "Creating our first Playbooks"
tags: [cybersecurity, ansible, automation, homelab]
# thumbnail image for post
feature_image: "images/2022-07-13/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-07-13
imagetag: "images/2022-07-13"
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


In our last post we talked about what Ansible is, how it can help us with our SysAdmin duties (both in the enterprise and in our HomeLab), and in today’s post we are going to talk about the continued set up and creating our first Playbooks.
<!--more-->
Yesterday we tested out our Ansible file by creating a dedicated project folder (called “Playbooks”) where we stored our “hosts” files. For testing out new hosts, or separating projects this would be a good practice, however since this is a Homelab and I won’t have to separate out my projects or hosts into a logical folder structure (such as for major enterprises/infrastructure setups):
![Folder layout](:/{{page.imgdate}}/2.png){:data-align="center"}

Let’s put our hosts in the default location (for Ansible), located at /etc/ansible/hosts. In Debian we have to manually create this folder:

```bash
$ sudo mkdir /etc/ansible
$ sudo nano /etc/ansible/hosts
```

Then we copy over the contents of our hosts file:

![Host File - Everything Copied Over](:/{{page.imgdate}}/3.png){:data-align="center"}

Now we can verify that Ansible is in fact reading this host file by default by running the command:

```bash
$ ansible-inventory --list -y
```

Which should produce this output:

![Ansible inventory](:/{{page.imgdate}}/4.png){:data-align="center"}

Then we can rerun our ping test again, this time we don’t call out our hosts file in our “playbooks” folder

```bash
$ ansible all -m ping
```

![Ahh, much shorter commands](:/{{page.imgdate}}/5.png){:data-align="center"}

Pretty cool right? Now you can run more than just “ping” which is a built in Ansible Command. You can run ad-hoc commands directly to the servers. Let’s try one (using “-a”). The bash command “pwd” will print the users default working directory, as if it just SSH’d into their environment:

```bash
$ ansible all -a "pwd"
```

![Printing Working Directory](:/{{page.imgdate}}/6.png){:data-align="center"}

Or another command

```bash
$ ansible all -a "df -h"
```

![Disk Space Usage](:/{{page.imgdate}}/7.png){:data-align="center"}

Remember in our Hosts file I had two blocks (one named “edgvm” and one named “splunk”).

![Two groups – edgevm and splunk](:/{{page.imgdate}}/8.png){:data-align="center"}

You can call these separately as well using Ansible Commands and it will only use commands in that particular group, lets try it:

```bash
$ ansible edgevm -a "df -h"
```

![Only Edgevm shows up](:/{{page.imgdate}}/9.png){:data-align="center"}

So why is this important that I am separating these two virtual machines in their own “blocks”? Simple, one is running Ubuntu and one is running RedHat Enterprise Linux, and if I was to try to run Ubuntu specific commands on RedHat I would get errors, and vice versa (such as apt-get update, or dnf update).

Onto our first Playbook. Let’s create our Playbook in /etc/ansible:

```bash
$ cd /etc/ansible
$ sudo touch playbook.yaml
$ sudo nano playbook.yaml
```

![First Playbook](:/{{page.imgdate}}/10.png){:data-align="center"}

Next let’s set up out “barebones” for our playbook:

```md
---
- hosts: all #we are calling all hosts here
  vars:
    - username: justin #Optional - it's in /etc/ansible/hosts
    - home:  /home/justin 
  tasks:
    - name: 
```

These should all be pretty self-explanatory. Let’s create a playbook that updates my Ubuntu virtual machines Apt Cache:

![Updating APT Cache](:/{{page.imgdate}}/11.png){:data-align="center"}

And let’s run the playbook, so while in the same directory (/etc/ansible).

```bash
$ ansible-playbook playbook.yaml --ask-become-pass
```

Let’s break down these commands

ansible-playbook : this is the initial command to run playbooks
playbook.yaml : this is the playbook file we just created
–ask-become-pass : This will prompt us for a become (su or root) password. This is so that we are not storing these in plain-texts

![Successful Playbook run!](:/{{page.imgdate}}/12.png){:data-align="center"}

And just like that we have created our first playbook, using pre-defined Ansible Commands (apt) in a playbook to update our apt cache for one of our VM’s.

Ansible provides a multitude of ways for System Admins to create playbooks to altar, change, define, and customize/control their infrastructure and environment and even launch their infrastructure as a code to develop highly available, very flexible environments, cutting down on tasks that would normally take hours/days to complete in no time.



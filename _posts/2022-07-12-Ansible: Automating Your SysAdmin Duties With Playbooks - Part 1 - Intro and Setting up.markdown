---
layout: post
title: "Ansible: Automating Your SysAdmin Duties With Playbooks - Part 1 - Intro and Setting up"
description: "Ansible can make day-to-day tedious System Administration/Engineering work a breeze"
tags: [cybersecurity, ansible, automation, homelab]
# thumbnail image for post
feature_image: "images/2022-07-12/feat-1.png"
# disable comments on this page
comments_disable: true
# publish date
date: 2022-07-12
imagetag: "images/2022-07-12"
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

[Ansible](https://www.ansible.com/resources/get-started) is a product currently developed by RedHat which is similar to [Chef](https://www.chef.io/) and [Puppet](https://puppet.com/), which provides SysAdmins a way to deploy, configure, and orchestrate their whole environment through the use of Playbooks (scripts). A major plus however to Ansible is that it 
<!--more-->
is agentless, meaning that there isn’t a need to install another piece of software on the remote systems in order for Ansible to communicate and interact with them. To make things even more simpler, [Ansible Tower](https://docs.ansible.com/ansible/2.5/reference_appendices/tower.html#:~:text=Ansible%20Tower%20(formerly%20'AWX',all%20of%20your%20automation%20tasks), provides a Web UI front end which can help expedite the deployment of these Playbooks, provide security (by allowing the sharing of credentials without the ability of transferring the credentials), as well as other many other cool features.
![Photo Screenshot from https://www.ansible.com/]({{page.imagetag}}/2.png){:data-align="center"}

(While outside the scope of this particular blog post, just for note: Ansible Tower is unfortunately behind a RedHat pay-wall, HOWEVER, the AWX Project – [here](https://www.ansible.com/products/awx-project/faq) and [here](https://github.com/ansible/awx) is open-source and upstream. It is readily available and can be used, however I would highly suggest learning how to write Playbooks first before trying out AWX/Ansible Tower in order to get the basics down first).

Ansible can make day-to-day tedious System Administration/Engineering work a breeze, especially for anyone Cybersecurity. Imagine you are responsible for the patching/remediating a Debian server that you must SSH into (be it is at home, or at work) due to a new found critical vulnerability. What would the steps be in this process?

```bash
$ ssh user@machine
Enter Password: xxxxxxx
Welcome to your Debian System 
(Enter the Message of the Day)
$ sudo apt-get update && sudo apt-get upgrade (Or sudo apt install xyz if updating a particular package)

(insert bunch of text involving updating and upgrading after you enter "Y")
```

Now imagine that you have five, ten, hundreds of servers, and now its Debian, Ubuntu, RHEL, Windows, OpenSUSE, Arch (BTW). Do you really want to go one by one to update these servers? By the time you finish these servers you will either:
A) Have to go back and redo it again due to the new vulnerabilities that come out by the time
or
B) Go Crazy and Quit because you are tired of typing
or worse
C) Make a mistake and ultimately bork a system…

Luckily, setting up Ansible is rather straight forward and the Playbooks are written out in a way that makes it easy to read (YAML – Yet Another Markup Language). Lets get started.

First lets fire up a Debian VM, this will be our “Control Node”, and update our APT Repository for good measure:

```bash
$ sudo apt update
```

Luckily for us, Ansible is in the default Debian repositories so we can easily install it with:

```bash
$ sudo apt install ansible
```

![Ansible installed]({{page.imagetag}}/2.png){:data-align="center"}

You can confirm everything is good with
```bash
$ ansible --version
```
![Ansible Version]({{page.imagetag}}/4.png){:data-align="center"}

And just like that Ansible is installed on our Control Node. Now how do we use it? Well Ansible works by communicating over SSH (primarily), API’s, or even WinRM (little harder to set up but can be done). Since I have other VM’s readily available with SSH, the rest of this series will be focused on connecting via SSH.

Lets create a new project directory to store our playbooks and go into that directory:

```bash
$ mkdir ~/playbooks
$ cd ~/playbooks
```

And we will make a hosts file, also known as an Inventory file. This file will contain the IP addresses of the hosts we will interact with using playbooks. (I use nano, sue me)

```bash
$ nano host
```

![New Host File]({{page.imagetag}}/4.png){:data-align="center"}

Inside of it:
[node1] – Identifies different “Groups”
10.10.10.21 – IP Address of one of the hosts (can have multiple lines of hosts)
ansible_ssh_user = xxxx – identifies the user to be logged into the machine – needs to be manually created on each machine.

Next lets create our SSH Keys and push them to our nodes (will need to repeat to each machine) – This is a one time procedure.

```bash
$ ssh-keygen -t rsa
```
Press enter mercilessly.

Now copy them to your machines (my above example is 10.10.10.21):

```bash
$ ssh-copy-id -i ~/.ssh/id_rsa.pub justin@10.10.10.21
```
![Adding the SSH keys to allow my Ansible Control Machine to talk to my other machine]({{page.imagetag}}/5.png){:data-align="center"}

So now that we have our hosts file (also known as the Inventory File), we will test our Ansible installation and setup to ensure that everything is working and communicating as it should. Just as junior network admins use ping, so does Ansible in their tools, lets use it now:

```bash
$ ansible -i ./hosts all -m ping
```
A good ping should look like:
![Ping Pong]({{page.imagetag}}/6.png){:data-align="center"}

Otherwise it may look like:

![Bad Pong]({{page.imagetag}}/7.png){:data-align="center"}

The possibilities with Ansible are nearly endless as you start to delve into learning how you can better utilize this in your environment. From simple patching to eventually setting up your entire Infrastructure as a Code (although other tools are available, such as Terraform, it can be done with Ansible!). In the upcoming days, we will continue to build playbooks to interact with our other virtual machines.
---
layout: post
title: "My Wins and Struggles: OpenStack Manila Dev Environment Set Up"
date: 2025-12-22
categories: [Outreachy, OpenStack, Internship]
author: "Rose Kimondo" 
tags: ["outreachy"] 
---

Hello, this is my progress blog post for the first two weeks of my internship and I am excited to share what I have been doing, my struggles, my wins, and above all, what I have learnt.  

* * *

## Week 1

Week One was from December 8th and I had my first meeting with my mentors [Goutham](https://github.com/gouthampacha) and [Carlos](https://github.com/silvacarloss). I was very happy to finally see them as we had only interacted via IRC and comments on code changes during the contribution phase.  
I also received my first action items which were setting up [DevStack](https://docs.openstack.org/contributors/code-and-documentation/devstack.html) on a VM that was provided by my mentors, and VS Code SCP.

### What is Devstack?

For development, you need to test the changes you develop and ensure that they function properly. OpenStack is a complicated ecosystem, making it necessary to verify the interoperability of the code changes with other plugins.  

DevStack provides an easy way to deploy an OpenStack cloud to do [functional and interoperability testing of changes](https://docs.openstack.org/contributors/code-and-documentation/devstack.html#:~:text=An%20important%20part,e.%20stable/pike)  

While the core services are installed, Manila's services aren't and you have to configure them. I will share the installation steps in a separate blog post.  

### What is VS Code SCP?

[VS Code scp](https://marketplace.visualstudio.com/items?itemName=aminkira.vscode-scp) is an extension that transfers files from a workspace to a remote server. In this case, from my local IDE to the DevStack we have just installed in the VM above. It uses SSH.  

One thing to note is that I tried working with the extension, but no changes were being pushed to my DevStack so I instead found a command that can be used in the terminal to achieve this:

``` bash
scp manila_ui/test_sync.txt stack@38.108.68.207:/opt/stack/manila-ui/manila_ui/
```

All in all, week one was not that hectic.  

* * *

## Week 2

Week 2 was from December 15th and was a bit challenging. I got two new action items: More environment set up to see changes on the dashboard (Horizon) and learning the Manila commands.

For the first task, I was provided with a video recording of previous interns setting up their environment. Here is the link: [Horizon/Manila-UI development environment](https://www.youtube.com/watch?v=AlSrgylBBlM)  

At this point, I noted that my Horizon dashboard looked different from what was being shared in the video. Two of the panels in the Project tab were showing errors when trying to load on my end and specifically Compute (Nova) was not loading up all its resources. I immediately knew that something had gone wrong during my DevStack installation in week one and decided to rectify it before making any progress so that it was not problematic in the future.  

The re-installation took me a while to finish as I kept running into an error surrounding network assignment, but after I deleted the user and created afresh, I was able to re-install.  

* * *

### Horizon + Manila Set Up

I then proceeded to follow the steps in this video. Basically, this is how to set up a development environment for Horizon (OpenStack Dashboard) with Manila UI (the Openstack Manila dashboard). The goal was to see the Manila UI plugin in the dashboard and see the changes that I make to the Manila UI code base reflect there in real time by loading it on my local machine. This is how I will be able to make the necessary improvements to Manila UI.  

Having DevStack correctly installed in your VM as I did in the steps above is very crucial, as your Horizon points to your DevStack's IP address.  

Well, the dashboard loaded well the first time, but then when I loaded the second time, the styling was completely off. So I tried again and again, but still nothing.  

With a little help from Gemini, I figured that I needed to change local_settings.py file in Horizon's codebase. I was getting 404 errors for stylesheet paths like /static/dashboard/css/output, meaning that the development server was looking for "compressed" versions of my style files but they did not exist in the static folder.  

I needed to tell Horizon to either generate these files or disable the compression so it serves the files "raw" for development. I went with the second option with these changes to local_settings.py.  

``` python
COMPRESS_ENABLED = False
COMPRESS_OFFLINE = False
```

However, the dashboard is loading slowly so I need to figure out how to speed it up (maybe the first option of generating the files), but that can wait for now. I am honestly scared to break it. Haha 😄😄  

* * *

I am planning to have blog posts that documents specifics of environment set ups and the steps that I have used to debug/solve any errors that I have come across.  This will be specific for my project so if you are keen on contributing to Manila UI or even Manila, I hope that these will be of help.  

In a separate post, I will also talk about the Manila structure and commands.  

Thank you for reading up to this point.  

* * *

---
layout: single
title:  "New Amplifi Alien Router"
date:   2019-12-05 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: I've recently been getting more and more complaints about the quality of the WiFI at home. Especially as there is more gaming, switch from Cable to Streaming TV, and the IoT device count is going up. I bought Amplifi Alien router and installed it. First impressions are...
#header:
#  teaser: /images/KinesisWithDotNet_DiagramOfKinesis.png
permalink: 
tags: [home, wifi, internet, router, LAN]
---

Like most technology industry employees I know, I am the technical support department for my house, mine and my wifes parents house, sometimes our siblings and neighbours too.
A long time ago I was quite good at this stuff.
I knew about about computer components, I setup networks, and I could fix peoples Windows installs.
However it's more complicated now and I do a lot less with real physical computing then I used to.

In my house I've recently been getting more and more complaints about the quality of the WiFI at home.
We have an increasing number of internet capable devices: Laptops, Phones, TVs, Streaming Sticks, Xboxs, Watches, Smart Plugs, Pool Equipment etc etc.

For a long time (10 years perhaps) I've had a Netgear router.
I changed models about 5 or 6 years ago to a Netgear Nighthawk R6700. 

![Nighthawk R6700 Image](../images/2019-12-05-new-amplifi-router/netgearr6700.jpg)

For the most part it was a pretty good router. We had a few phones, some laptops, and an Xbox 360 which needed access to our 25Mbps internet connection.

Since then we have moved house, which included changing our Comcast Cable to AT&T Fiber 1Gpbs internet. 
We have also had more children ago into phones, tablets, and gaming consoles. 
I have got more smart devices like home automation, thermostats, video doorbells, pool equipment, and security devices like cameras.
I also switched from DirecTV to YouTube TV, meaning every second of TV and Movies is streamed in at least HD.

## Nighthawk Features

For the whole time I had both my Netgear routers they had some cool features. 

1. They could both have seperate 2Ghz and 5Ghz networks.
I used this feature, which might be stupid now, but originally I had a device or two that couldn't connected to a single network that had both bands. Possibly because my orignal Netgear was a pre-standard N router. 
1. They both supported a Guest network, which I mainly leave off, but it's useful if you have strangers you want to give internet to, like in a cafe or store environment.
2. They could both have a USB hard drive connected and expose it as a network share to all my windows devices. I used to use this a lot before I went to Build and got a free 100GB OneDrive account
3. The R6700 has built in Quality of Service routing
4. The R6700 built in Parental Controls. This started out as pure DNS filtering but through a software update maybe 2 years ago, became Disney Circle.
5. The R6700 also supports VPN connections so I could connect back to my home network from anywhere. 
6. It supports DynaDNS to make the VPN much easier to use.

So why would I get rid of this great router? 2 reasons:

### Signal Reach 
I had already moved it to the center of the middle floor in my house to improve reach to everywhere. I did this with Powerline Ethernet (not recommended when crossing breakers) and recently did it with Coax based Ethernet (works really well as we dumped Cable TV).

I don't know if the number of devices on the network caused a problem or if it was the construction of my house, or maybe interference of other devices, but I couldn't get this to be good enough.

### Reliability

On a weekly, sometimes daily basis, the router would stop broadcasting it's signal and everything woudl disconnect. This required me to go to it and do a physical reboot.

One time even the router rebooted itself and forget my ENTIRE configuration. My two networks, passwords, circle profiles, everything. It started broadcasting it's default SSID and password and I had to connect from scratch.
This takes time and is quite annoying, especially when you were in the middle of streaming a show and everyone thinks it's your failure to manage it properly somehow. 

Even when the network was working, it would have minor drop outs. 
Running a continual Ping would highlight that for some devices, some times, the network would drop for maybe between 2 and 10 seconds, then come back again.

## Hunt for a new Router
Reliability ended up being the straw 'that broke the camels back' so to speak. So I went on the hunt for a new router. 

I wanted something that could give access to my Gigabit home internet as reliably as possibly. That was the primary concern. My secondary concern was being able VPN in to my network, and thirdly to continue to provide the management that I got with my Nighthawk. 

I read quite a lot of reviews of different types of WiFi solutions, including new Netgears with more ariels, Google WiFi, other mesh technologies. 

I originally turned off from Netgear because of the reliability factor. 
I made the assumption that the problem with the old one was the software updates. 
While they brought good features, I didn't think they were as tested as they should have been. 
Perhaps this would be different now on newer hardware, but new hardware will eventually be old and I think the trust was broken. 

At the point I was finishing up search without a clear choice, people in my Twitter stream started posting about the Amplifi Alien router.
A freshly released, super strong signal, super duper fast Wifi router.
The timing was perfect, I was frustrated, in need of change, and here it was with ringing endorsements from people like [Scott Hunter](https://twitter.com/coolcsh/status/1198400265042292736) and his followers.

I went online and ordered one. 
It wasn't cheap - $379. 
I waited for it to come. 
And it did.
Today.

## AmpliFi ALIEN

![ALIEN Router](../images/2019-12-05-new-amplifi-router/AFi-ALN-R_Front_1024x1024.png)
The ALIEN router was packaged very nicely, and very simple to setup.
Plug in the Ethernet cable from your ISP router (mine was from the Coax adapter) and plug in the power. 
Download the App from your phone's App Store and it handles the setup pretty well. 
Once you have it setup it has a nice app, and a nice display on the front. 

These are the features I have used so far:

### WiFi Speed

So far the speed on the WiFi is **much** faster than the Netgear was. 
I'll admit that I don't have the same number of devices connected to it just yet. 
My plan was to leave my smart home devices on the old Netgear for a short period to see if it could be reliable with less devices and lower throughput.
If that fails, I'll go through the painful process on moving the IoT stuff over. 

My clearest example of a speed difference is my son's (oldish) Macbook Air.
He used it in the same room as the Netgear and consistently got about 80+Mbps download and upload. One the ALIEN it gets 330+Mbps download and about 600+Mbps upload. (I dont know why they're different)

### Profiles

The app allows you to create profiles for people in your family. You type in someones name and can assign devices to their profile. 
This is similar to how Disney Circle works, but the feature here is limited to pausing someones internet immediately, or setting up 'Quiet Time' for them, so they dont use their device in the night.
I chose a different (combined) network name this time so the devices have't all auto-reconnected.
This has given me the opportunity/chore of being able to/having to connected them one by one.

It does also allow setting better names for devices too.
My Nest thermostats have very obscure GUID like names on the network. 
As I added them I was able to rename them in the App to be Upstairs Nest and Downstairs Nest.

You can also tell the router to Optmize for Streaming or for Gaming too for each device. 
So I can set my Roku TV to Streaming and my Xbox One to gaming. 
I'm not sure what the effect will be during real use but it seems cool so far.


It doesn't have the content filtering capability that Disney Circle has. 
Therefore I need/want to replace that feature in my house. 

## Summary

So far the ALIEN router is a good WiFi router for the devices I have connected to it. 

The one feature it's missing is proper parental controls. 
It allows you to disable internet for certain profiles, but doesnt do content filtering like Disney Circle does. 

I am likely to buy a Circle device to replace that feature set if I can't find a Raspberry Pi based solution.

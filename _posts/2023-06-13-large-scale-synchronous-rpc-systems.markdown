---
layout: single
title:  "Synchronous Communication and large scale systems"
date:   2023-06-03 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: A discussion of how synchronous communication at scale and what it might take to be successful
#header:
#teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [tech, architecture, synchronous, rpc]
---

Plenty of my posts, vidoes, and mentoring have been about asynchronous communication and how important it is for building large scale and complex business systems.
However, since joining Google a year ago I have seen the scale and reliability that they have achieve in synchronous RPC based systems design, so I thought it would be valuable to talk about that too. 

Synchronous RPC is a powerful tool that can be used to build large scale systems.
Google is a great example of that. 
However, it is important to understand the challenges involved in building such systems and what it takes to overcome them.

## Redundancy

One of the biggest challenges is **redundancy**. In a large scale system, there is always the possibility of a failure. 
This could be a hardware failure, a software failure, or a network failure. To ensure that your system remains available, you need to implement redundancy. 
This typically involves having multiple servers for each service, and using a load balancer to distribute traffic across multiple servers. The more reliable you want to be, the more servers you need, both hosting the business services, and also the technical services like the load balancers, the databases, the identity systems. Managing a large deployment like this is complicated. 

Having many redundant services on different services presents another problem for the system. **Service location** is another important consideration. 
Each of our services need to be able to find the other services that it depends on. This can be done using a service registry.
A service registry is a central repository that maintains a list of all the services in the system.
The service registry needs to have a up to date information about all the services that are available (running and healthy) in the overall system, and needs to be able to provide information about them on demand.

## Automation
Another challenge is **automation**. In a these large scale systems, with all the servers and services that need to be deployed, there is a lot of manual work that needs to be done. This could include tasks such as provisioning servers, deploying applications, and monitoring performance. To reduce the amount of manual work to a level that is achievable by a reasonable size team of people, you need to automate as much of the process as possible.
At really big scale it means more than just scripting, it means having real tooling for automation of running deployments, monitoring the progress, handling rollbacks etc. 

## Monitoring and observability

**Monitoring and observability** are essential for ensuring that your system is healthy and running smoothly.
Monitoring involves collecting data about the system, such as CPU usage, memory usage, and network traffic.
Observability involves using this data to understand how the system is behaving. 
Monitoring your services and servers is required in order to keep your service registry up to date.
It's also important to understand whether your overall system is actually operating.
You need to understand what components are essential for functionality, and whether you have enough of each component reachable in order to provide it.
Monitoring should feedback into the automation.
Being able to automatically restart a services, or a server can help systems recover from problems. 
Being able to react to high utilization of servers and turn on new servers and start-up new instances can help you react to high demand.

## Summary
Building large scale systems with synchronous RPC can be challenging, but it is possible.
Infact, some of the biggest systems we use daily are using these techniques successfully. 
However, it's not without complexity and needs a lot of investment in supporting technology to make it work.
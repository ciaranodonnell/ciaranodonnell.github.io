---
layout: single
title:  "Are Microservices and Containers the same thing"
date:   2020-02-21 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Microservices are often thought as the modern way to build cloud native applications, so are containers. The question addressed here is whether microservices means containers, or containers means microservices. 
#header:
#  teaser: /images/KinesisWithDotNet_DiagramOfKinesis.png
permalink: 
tags: [microservices, containers, architecture, software engineering]
---

Microservices are often thought as the modern way to build cloud native applications.
A thousand conference talks have been dedicated to talking about microservices, and how we build them, and how to succeed with them.
I have seen a **lot** of these talks, and in my small circle so far, I have given a few too.

## This always happens

The microservices trend in the industry has suffered from a recurring problem: Architecture Princples get conflated with implementing technology.
It has happened before with Service Oriented Architecture. We had technology venders pushing a specific platform as the way to do SOA. Enterprise Service Buses and Business Process Management/Orchestration tools became the default assumption for what that technology meant.
The end of the this processes is that people start to think that implementing the technology is the same as implementing the pattern. There are a lot of enterprises that have ESBs and BPM tools and believe they are using SOA, even though they don't have a coherrent understanding on the SOA pattern.

Similarly with microservices people are starting to associate Containers, Docker, and Kubernetes with microservices. We are already arriving at the stage where people assume that microservices are deployed in containers, and very soon we'll see a common pattern of people thinking that if they have containers, they are doing microservices already.

## So what are microservices?

I try and explain this to my team, and other teams a lot. I like to give people an extreme view to illustrate a point. I say:
> There is no such thing as 'a microservice'!
> 
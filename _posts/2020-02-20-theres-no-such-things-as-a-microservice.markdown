---
layout: single
title:  "There's no such thing as a microservice"
date:   2020-02-21 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Microservices are often thought as the modern way to build cloud native applications, so are containers. The question addressed here is whether microservices means containers, or containers means microservices. 
#header:
teaser: /images/Gartner_Hype_Cycle.svg
permalink: 
tags: [microservices, containers, architecture, software engineering]
---

Microservices are often thought as the modern way to build cloud native applications.
A thousand conference talks have been dedicated to talking about microservices, and how we build them, and how to succeed with them.
I have seen a **lot** of these talks, and in my small circle so far, I have given a few too.

## Hype cycle of technology

![Hype Cycle Image from Wikipedia https://en.wikipedia.org/wiki/Hype_cycle](/images/Gartner_Hype_Cycle.svg)

The hype cycle idea has been used to describe the pattern in which (successful) new technologies are adopted by technologists.

To begin with there is a *Technology Trigger* - the new thing that happens. This can be a pattern like microservices, or a technology like Containers or Kubernetes.

This new technology starts to generate interest and as that builds, there is a lot of hype around it. People start describing it as the panacea, the next big this, a game changer. This is the *peak of inflated expectations*.

Once people try and adopt the new technology, they start to become disappointed that it isn't magic. It doesn't change the world the way the hype promised. This is the *trough of disillusionment*.

Then as the technology stabilizes and matures people start to figure out what it's truly good for.
This is the *Slope of Enlightenment*.
Some technologies don't reach this phase and they fall out of favor.
This is a problem for the people that implemented them and now have these systems built on them.

Over time technology settles into long term use, the *Plateau of Productivity*. This is the long term state of successful use of software.

### Technology Vendors in the Hype

The Peak of Inflated Expectations has a lot of causes
Celebrity endorsements is one, which is technology means people talking about them at conferences. People then see this as the new conference topic and every conference wants these talks and everyone wants to give them. 
This is very much whats happened with microservices recently, but happened with SOA, NoSQL, and API first patterns before.

Some of these technologies, like NoSQL, are inherently vendor driven.
NoSQL is an idea popularized by product vendors.
The first I really heard of was MongoDB, which wasn't just presented at general conferences, but actually had its own one, MongoWorld.

Other trends are more like architectural styles, like SOA and microservices.
Once these things become conference cool, they actually attract vendors to them.
API Gateway is the name of a design pattern, but there is now a huge number of vendors that are selling API Gateway and API Management products.
These products now do more than just the pattern, they provide features around Authentication and Authorization, Flow Management, Protocol Translation, Versioning, Scaling, and all kinds of other related features that can pad out their comparison sheets.

I generally like the fact these products make life easier for us.
Writing an API Management layer is hard, so products are good.
The challenge we as an industry have with this hype cycle pattern is when people start to conflate the product with the patterns.
It's quite hard to talk to people about API Gateways without people defaulting to thinking about their favorite product.
Similarly it was hard to get people to talk about their SOA strategy without them thinking about their ESB or BPM tool that did it for them.

The microservices trend in the industry has suffered from a recurring problem: Architecture Principles get conflated with implementing technology.
It has happened before with Service Oriented Architecture. We had technology venders pushing a specific platform as the way to do SOA. Enterprise Service Buses and Business Process Management/Orchestration tools became the default assumption for what that technology meant.
The end of the this processes is that people start to think that implementing the technology is the same as implementing the pattern. There are a lot of enterprises that have ESBs and BPM tools and believe they are using SOA, even though they don't have a coherent understanding on the SOA pattern.

Similarly with microservices people are starting to associate Containers, Docker, and Kubernetes with microservices. We are already arriving at the stage where people assume that microservices are deployed in containers, and very soon we'll see a common pattern of people thinking that if they have containers, they are doing microservices already.

## So what do we mean by microservices?

I try and explain this to many people, quite frequently, and in order to make people understand it I say:

**There is no such thing as 'a microservice'!**

Microservices is not a thing you build, it's an architectural style.
We build microservices architectures like we built layered architectures in the past.
People didnt claim to have built a 'layered', or a SOA.
People built services, and they still build services.
The way they go about designing them is different with microservices. The way we draw the lines between our services is different, and the principles we have around uptime management, data duplication, hosting, and support are different. But we are still building services.

### Principles of microservices style architectures

Microservices as an architectural style has principles. We are starting to get pretty common lists of principles, and now even patterns which are used in microservices architectures. The core list of attributes of this type of architecture is:

1. Business Oriented Services
2. Highly Cohesive
3. Loosely Coupling
4. Independently Deployable
5. Owned by a small team
  
We have other related things that we see microservices architectures frequently use too:

- Continuous Delivery
- Automated Deployment
- Automated Testing
- Agile Product delivery methodology
- Database per service
- Event based communication

These are all principles and patterns for microservices, and there are others that I haven't listed.

Notice that in this list I haven't mentioned: Docker, Containers, Kubernetes, Functions/Lambdas, or any other implementation technology.
Because that isn't what microservices is about.
It's about the thought process you use to design the overall software you're building.

In my personal view the most important aspect of a microservice is that it's oriented around a single business capability, often a departments role in the larger system.
We can break down larger business models into smaller sections using Domain Driven Design.
We 'decompose' the business domain into these sub-domains/bounded contexts and we can use that as our guidelines for the services we need to build.
We cant then follow our other principles of loose coupling, and independent deployability to deliver scalable, resilient services.

### What microservices aren't

So microservices isn't a thing you build or deploy.
Microservices is a way of designing a service, that you then build and deploy.

A microservice isn't a container in a containerized application, whatever the orchestrator that runs it, or the cloud it runs in. Containers are a hosting model, like a lightweight VM. I could take any monolithic application, that hasn't been decomposed to bounded contexts, that has low cohesion and lots of coupling, is developed by any number of teams, and run that in a container. Being the container won't make it a microservice.

Now I'll be honest, I do often call services, that I have developed in a microservices way, a microservice. I do this as shorthand to show the architectural mindset I used to design it, and often the operational model I envision to continue it's development long term.

## Get more information

As I was finishing this article I was talking with colleagues about the idea and I said I planned to publish this and perhaps make a talk titled the same. She put the title into google and discovered that I am not the first person to argue this point, or use this title for it. 

Chris Richardson of [microservices.io](https://microservices.io) excellence actually has a really detailed and fantastic talk on this topic which I recommend EVERYONE to go and watch:

[There's no such thing as a microservice - Chris Richardson](https://www.youtube.com/watch?v=sL320gRD2dU)

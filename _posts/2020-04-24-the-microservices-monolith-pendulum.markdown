---
layout: single
title:  "Monoliths to Microservices! And back again?"
date:   2020-02-21 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Monoliths were once all the rage, but recently microservices have become the target state for everyone who wants to be trendy. Now it seems people are bucking the trend and moving back to monoliths. Here I think about why that might be  
#header:
#teaser: /images/Gartner_Hype_Cycle.svg
permalink: 
tags: [microservices,monoliths, architecture, software engineering]
---

# Monolith to Microservices, and back again?

Microservices is **the** cool architecture buzzword of the last 5 years.
There are thousands of blog posts, conference talks, and books that dig into microservices in different ways.
A lot of these talks and books focus on the core principles that define microservices, and lots of them are fairly common.
In training that I created for Avanade I summed the important characteristics up as:

1. Highly Cohesive Components
2. Organized around business capabilities
3. Autonomous
4. Designed for Failure
5. Smart endpoint, dumb pipes
6. Use Infrastructure Automation

These principles are pretty well known now and lots of people talk about them, or a similar list when advising on microservices.

The challenge with them is that they are a little abstract.
I could write a similar set of rules about automobiles needing wheels, seats, engines, windows, being made of metal, burning gas, etc.
I could then follow all of those and still get hatchbacks, mini-vans, ferraris, buses, and Hummers.
All very different vehicles with different good points and bad points.

I think something similar to this has happened with microservices.
Lots of people have followed the guidelines in different ways, and they end up with something unique.
Sometimes they are Hummers, all expensive to run, and sometimes theyre like little japanese hatchbacks, efficient to run but not as fast or powerful.
Everyone then talks about the pros and cons of microservices, debating the various points, but talking about different things.

Now I am not the original inventor of the term microservice, so I don't claim to know what their intention was, nor what the exact problems they were solving that caused them to create microservices.
But, I have written systems this way, evolving my own path to microservices style architectures through my career.
In my current role I am tasked with leading software architecture for a consultancy, and therefore I see lots of problems and solutions being designed and built using microservices approaches.

I have refinements, or perhaps more specific guidelines one designing microservices for enterprise systems.
I say enterprise systems as I don't often solve problems like Twitter, or Netflix, where its a relatively simple business, but a large technical challenge around scale, performance, or throughput.

## Designing a real Microservice

### Components

This is really where the micro part of the microservices names comes into play.
If figuring out the scope of the various components that gets people caught up when trying to figure out what microservices means.
This was a similar challenge during the SOA era in the industry and I think it stems actually from peoples understanding of what a service is in the SOA and microservices world.

Wikipedia describes a service as:

> the term service refers to a software functionality or a set of software functionalities (such as the retrieval of specified information or the execution of a set of operations) with a purpose that different clients can reuse for different purposes

I think thats the part that most people stop reading.
A service is a grouping of functionality that can be reused. Cool!
Then a lot of developers, who are experts in their implementation of a system, naturally group functionality up following how theyve done it before - as similar implementations in their specific layer.
In the SOA world, and in the mciroservices world now, this gives us LOTS of very small services, deployed independently, which can be assembled together to make a functional whole.

If we read a little further through that same Wikipedia page, under the *Service Engineering* section we'd find:

> A business analyst, domain expert, and/or enterprise architecture team will develop the organization's service model first by defining the top level business functions.
> Once the business functions are defined, they are further partitioned and refined into services that represent the processes and activities needed to manage the assets of the organization in their various states.
> One example is the separation of the business function "Manage Orders" into services such as "Create Order," "Fulfill Order," "Ship Order," "Invoice Order" and "Cancel/Update Order."
> These business functions have to have a granularity that is adequate in the given project and domain context.

Ok, so thats a little more detailed.
Importantly here is says the first step is to decompose the business into seperate top level business functions.
I agree with that, decomposition by business domain/bounded context makes sense.

But as we keep reading it tells us that these domains then need to be decomposed further into seperate activities, or workflows, that each business domain contains.
To me this is possibly a step too far.
Making each serivce a specific activity means that overall these services (or microservices) arent completely functional.
Yes, they do a thing, but they arent really re-usable on their own.
I need re-use all of them to make it all re-usable.

So to sum up, a service is a grouping of **business functionality**, that is **small enough to be a single piece of business functionality**, but **large enough to be re-usable**.

This is actually what our 2nd bullet means.
Our microservices should be organized around business capabilties.

### Autonomous

Well this is a pretty abstract term for getting people to understand service design.
What on earth does it mean?

When we talk about Autonomy in microservices, what we really mean is that the service needs to be able to operate on its own.
It should be a complete unit that doesnt need other microservices to be functional.

  (
      *Worth noting here that we're talking about other services, not infrastructure.
  It's normal for our services to need infrastructure, like databases, or message brokers in order to.*
  )

Firstly, I think this reinforces out definition from before about them being a complete bit of business functionality, like all the activities to manage orders.
If we broke microservices down to individual activities then they would no longer be independent.
They would need to share a datastore that contains the orders, and could potentially need to talk to each other in real time in order to operate.
In the wikipedia example, Invoice/Ship/Fulfill order would probably all need to call the Update order function to persist their changes.

Now, I think this Autonomy concept is where a lot of people miss some of they design elements of successful microservices.

### Dumb Pipes and Designing for Failure

- Designing for failure is hard.
- Falacies of distributed systems
- lots of people chose to go with the simplest pipe - ReST
- REST means lots of things, like service discovery and temporal coupling.

## The pendulum

I'm seeing an increasing number of anti microservices posts recently.


## Why did it go wrong?

I still think, like SOA, most people trying it don't understand it.
They try to size them and group them on technology lines.
They see them as totally different to monoliths.
I don't see them that way

## Back to Autonomy

I think of microservices as being a thought pattern.
What you actually build is systems.
Each component of the larger system is like a smaller system that handles a single piece of the business domain.
So it's like a mini monolith that has a small scope.

Each of these component systems (microservices) is like a full stack app with its own little domain, storage, etc.
It's really like a small monolith.

What makes this work that that these smaller monoliths need to really be independent.
Which means they shouldn't talk synchronously to each other to perform big distributed transactions.
They should have their own smaller transactions they can perform.

The larger system operates by putting these smaller Transactions together in larger sagas following that pattern.
These should be asynchronous and brokered.
Like event driven architectures.
The component systems /mini monoliths should publish integration events.

That's how other monoliths can get information they need to do their transactions.
These could be product summaries or currency reference data or whatever.
They should get the events and store the data they need.

That's how I model systems built of many smaller autonomous parts (microservices).
I hear a lot of people who break their systems down to too smaller parts.
Transactions are modeled across them in a synchronous fashion.
That's how they get so complex and stay fragile.

IMO Microservices is a mental model that limits the business scope of your monoliths to 1 bounded context.
It then gives you a set of principles for making your smaller monoliths interact while staying reliable.

## So the pendulum is a symptom of poor boundary control

That's why I think there's this back and forth now about monoliths arent so bad.
It's right.
They get back when they grow beyond a single business area.
Microservices means stopping that from happening.
All the other serverless, containers, API gateways, etc are just noise

## Summary

So really everyone's kinda right, except for all the wrong things.

Monolith in its purest/worst definition is a single component with multiple business responsibilities.
Microservices is a thought pattern which gives you a model to build complex, multiple business responsibility software, where no individual component has more than one responsibility.
The services, or applications, that we build at the end of a microservices design should look like little monoliths.
They should have their own databases, full of all the data they need to operate. They shouldn't rely on other systems in real time, but integrate though standarized interfaces.

People are crazy that rationality of monoliths because they over decomposed when they went to microservices.

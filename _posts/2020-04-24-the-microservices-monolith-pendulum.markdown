---
layout: single
title:  "Monoliths to Microservices! And Back Again?"
date:   2020-05-29 18:43:00 -0500
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

# Monolith to Microservices, and Back Again?

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
I could then follow all of those and still get hatchbacks, mini-vans, Ferraris, buses, and Hummers.
All vastly different vehicles with different good points and bad points.

I think something similar to this has happened with microservices.
Lots of people have followed the guidelines in different ways, and they end up with something unique.
Sometimes they are Hummers, all expensive to run, and sometimes they're like little Japanese hatchbacks, efficient to run but not as fast or powerful.
Everyone then talks about the pros and cons of microservices, debating the various points, but talking about different things.

Now I am not the original inventor of the term microservice, so I don't claim to know what their intention was, nor what the exact problems they were solving that caused them to create microservices.
But I have written systems this way, evolving my own path to microservices style architectures, through my career.
In my current role I am tasked with leading software architecture for a consultancy, and therefore I see lots of problems and solutions being designed and built using microservices approaches.

I have refinements, or perhaps more specific guidelines one designing microservices for enterprise systems.
I say enterprise systems as I don't often solve problems like Twitter, or Netflix, where it’s a relatively simple business, but a large technical challenge around scale, performance, or throughput.
Enterprises suffer from existing systems, coupled integrations, complex business models with associated politics.

## Designing a real Microservice

### Components

This is really where the micro part of the microservices names comes into play.
If figuring out the scope of the various components that gets people caught up when trying to figure out what microservices means.
This was a similar challenge during the SOA era in the industry and I think it stems actually from peoples understanding of what a service is in the SOA and microservices world.

Wikipedia describes a service as:

> the term service refers to a software functionality or a set of software functionalities (such as the retrieval of specified information or the execution of a set of operations) with a purpose that different clients can reuse for different purposes

I think that's the part that most people stop reading.
A service is a grouping of functionality that can be reused. Cool!
Then a lot of developers, who are experts in their implementation of a system, naturally group functionality up following how they've done it before - as similar implementations in their specific layer.
In the SOA world, and in the microservices world now, this gives us LOTS of very small services, deployed independently, which can be assembled together to make a functional whole.

If we read a little further through that same Wikipedia page, under the *Service Engineering* section we'd find:

> A business analyst, domain expert, and/or enterprise architecture team will develop the organization's service model first by defining the top-level business functions.
> Once the business functions are defined, they are further partitioned and refined into services that represent the processes and activities needed to manage the assets of the organization in their various states.
> One example is the separation of the business function "Manage Orders" into services such as "Create Order," "Fulfill Order," "Ship Order," "Invoice Order" and "Cancel/Update Order."
> These business functions have to have a granularity that is adequate in the given project and domain context.

Ok, so that's a little more detailed.
Importantly here is says the first step is to decompose the business into separate top-level business functions.
I agree with that, decomposition by business domain/bounded context makes sense.

But as we keep reading it tells us that these domains then need to be decomposed further into separate activities, or workflows, that each business domain contains.
To me this is possibly a step too far.
Making each service a specific activity means that overall, these services (or microservices) aren't completely functional.
Yes, they do a thing, but they aren't really re-usable on their own.
I need re-use all of them to make it all re-usable.

So, to sum up, a service is a grouping of **business functionality**, that is **small enough to be a single piece of business functionality**, but **large enough to be re-usable**.

This is actually what our 2nd bullet means.
Our microservices should be organized around business capabilities.

### Autonomous

Well this is a pretty abstract term for getting people to understand service design.
What on earth does it mean?

When we talk about Autonomy in microservices, what we really mean is that the service needs to be able to operate on its own.
It should be a complete unit that doesn't need other microservices to be functional.

  (
      *It's worth noting here that we're talking about other business services, not infrastructure.
  It's normal for our services to need infrastructure, like databases, or message brokers in order to function.*
  )

Firstly, I think this reinforces out definition from before about them being a complete bit of business functionality, like all the activities to manage orders.
If we broke microservices down to individual activities, then they would no longer be independent.
They would need to share a datastore that contains the orders and could potentially need to talk to each other in real time in order to operate.
In the Wikipedia example, Invoice/Ship/Fulfill order would probably all need to call the Update order function to persist their changes.

Now, I think this Autonomy concept is where a lot of people miss some of they design elements of successful microservices.

### Dumb Pipes and Designing for Failure

Another important aspect of microservices is that we should design them for failure.
Thinking about [the fallacies of distributed computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing), we know that the network is NOT reliable, and latency is NOT zero.
Knowing those things, we quickly realize that in order for our software to be high quality, we need to handle the failure in the network, and the high latency periods that look like failure, but sometimes aren't.

Designing for failure is challenging though. It's very hard to make software continue to work when things it needs aren't available.
As I have written before, some (if not all) of these cases will need to be handled by **business** rules.
Most failures actually need to be thought about in terms of how they affect the business processes that are being handled.

One of the exacerbating problems with designing for failures is the interpretation a lot of people use for the 'Dumb Pipes' principle.

The dumbest pipe people seem to choose is REST over HTTP.
HTTP certainly qualifies as a dumb pipe, but it might actually be too dumb.
While REST is pretty good for getting across the entire internet from a user’s web browser or mobile phone to your services and back again, I think we need better for our microservices.

On the internet there are a lot of services provided for you, one big one being the DNS system.
You don't have to worry about people finding you and getting your address, you just buy a domain name and that system manages it for you.
You might also have some redundancy on the front door, perhaps something like Azure Traffic Manager or some other load balancer.

Internally with HTTP you have to manage all that, the service discovery and the load balancing.
This is complicated and takes a separate effort that doesn't provide any real business value directly, except for the fact it makes it work and done well can give you reliability.

REST over HTTP also keeps us coupled.
Temporal coupling means we have to make sure client and server / requestor and responder are running at the same time.
That's easy to do until things fail, or get slow, then it becomes quite a pain. Another complexity that needs to be managed.

## The pendulum

I'm seeing an increasing number of anti-microservices posts recently.
Some of these are from people that never moved, like David Heinemeier Hansson who wrote and interesting piece called '[The Majestic Monolith](https://m.signalvnoise.com/the-majestic-monolith/)'.
[Jet.com also blogged](https://www.infoq.com/news/2019/02/migrate-microservices-workflows/) on how they abandoned microservices to move to, what they call, workflows.
Segment moved [to microservices and back again](https://www.infoq.com/news/2020/04/microservices-back-again/) and wrote about it.

I've also seen various posts on reddit and tweets on twitter which have been talking about a growing discontent with microservices and arguing against their use.
A decent number of these people are arguing that we shouldn't be making this move to microservices at all.

There are lots of people in the world that build software and many of them will have made a move towards microservices.
I would also bet that a few of them are having bad experiences.

They are seeing increasing complexity managing a production environment with lots of moving parts.

They are struggling to manage the inter-process communication complexity that comes with having all these separate moving pieces that need to talk to each other.

They are also struggling to manage the increase in code volume. Each service needs scaffolding, configuration, deployment scripts, database scripts, etc. There are lot more lines of C#/Java/Javascript/YAML/ARM/Terraform/SQL to be managed.

Lots of people are seeing these increases in cost and complexity, like a tax on their microservices, but they aren't necessarily seeing the benefits: the scale, the resilience, and the supposed reduce complexity.

## Why did it go wrong?

I still think, like SOA, most people trying it don't understand it.
They don't understand what it's good for, and what it's bad for.
They don't understand how to decompose their existing systems and size their microservices.
They struggle to get the automation level, and observability they need to make it all manageable.

There are a lot of blog posts about the success, and the wins bug companies have had with microservices, but the people in existing enterprises don't operate in the same environment.

Most of corporate America have an IT department that grew up in the older way of thinking, before DevOps, before cloud, in a period where software releases were very infrequent, and production stability was paramount.
Developers couldn't be trusted to take care of IT, it needed to be protected from them at all costs.
Lots of the companies boasting big success stories aren't like that.
They are “pure digital” companies/software-first companies that go build instead of buy (and have done so with nearly all of their IT estate).

Corporate America is largely still on the opposite side of the spectrum (and “buy” 95% of the time and consider “build” to be systems integration work).
These digital first companies with their microservices success stories often have fairly limited scopes. They don't have the business complexity that most of corporate America operates.
They are web start-ups, with specific products.
So when they break down their monolith to small components, they sound like VERY small components to corporate developers.

Another massive challenge are software product companies (like Salesforce, SAP, Microsoft) that have a vested interest in discouraging decoupling.
The level of discipline and shared vision enterprises require to increase their technical agility is very high.

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
These should be asynchronous and brokered, like event driven architectures.
The component systems/mini monoliths should publish integration events.

That's how other monoliths can get information they need to do their transactions.
These could be product summaries or currency reference data or whatever.
They should get the events and store the data they need.

That's how I model systems built of many smaller autonomous parts (microservices).
I hear a lot of people who break their systems down to too smaller parts.
Transactions are modeled across them in a synchronous fashion.
That's how they get so complex and stay fragile.
They don't decouple, they don't achieve autonomy, they get the high-latency/distributed monolith.

I see Microservices as a mental model that limits the business scope of your monoliths to 1 bounded context.
It then gives you a set of principles for making your smaller monoliths interact while staying reliable.

### Culture is important!

Breaking the application up to separate components is critical.
Along with that is the separation of the teams that go along to support that.

Conway's Law is the key tenet we follow when we think about the changes the microservices bring to a development organization.
Melvin Conway wrote in [How Committees Invent in 1968](http://www.melconway.com/research/committees.html):

> organizations which design systems (in the broad sense used here) are constrained to produce designs which are copies of the communication structures of these organizations.

So as we design software that is divided along these business responsibilities, we need to shape our development teams to match it, **from the beginning**.

This change is very often forgotten when people move to microservices.
Or at least delayed, with people thinking they'll get there.

Failing to model our teams correctly will allow this Law to force us to write monoliths again.
It will affect how we do things like sprint planning, demos, and testing. It will affect the way we build interfaces and APIs.
It will also mean that the developers we have on our teams have too much mental context to maintain when developing.
They will be generalists in the whole business domain, never experts in their bounded context.

## So the pendulum is a symptom of poor boundary control

That's why I think there's this back and forth now about monoliths aren't so bad.
It's right.
They get back when they grow beyond a single business area.
Microservices means stopping that from happening.
All the other serverless, containers, API gateways, etc. are just noise

## Summary

So really everyone's kinda right, except for all the wrong things.

Monolith in its purest/worst definition is a single component with multiple business responsibilities.
Microservices is a thought pattern which gives you a model to build complex, multiple business responsibility software, where no individual component has responsibility for more than one business area.
The services, or applications, that we build at the end of a microservices design should look like little monoliths.
They should have their own databases, full of all the data they need to operate. They shouldn't rely on other systems in real time but integrate though standardized interfaces.

People advocate for the rationality of monoliths because they over decomposed when they went to microservices and created a high-latency version of the mess they had before.
They reject the pattern not because of their experience with it, but because they never followed it in the first place.

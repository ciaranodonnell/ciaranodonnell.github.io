---
layout: single
title:  "Monoliths to Microservices! And back again?"
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

# Monolith to Microservices, and back again?

Microservices is **the** cool architecture buzzword of the last 5 years.
There are thousands of blog posts, conference talks, and books that dig into microservices in different ways.
I think there are a lot of different takes about what it really means.
I have mine, you might have yours, and lots of luminaries like Sam Newman, Martin Fowler and others have theirs. 


I'm seeing an increasing number of anti microservices posts recently.
I still think, like SOA, most people trying it don't understand it.
They try to size them and group them on technology lines.
They see them as totally different to monoliths.
I don't see them that way

I think of microservices as being a thought pattern. What you actually build is systems. Each component of the larger system is like a smaller system that handles a single piece of the business domain. So it's like a mini monolith that has a small scope. 

Each of these component systems (microservices) is like a full stack app with its own little domain, storage, etc. It's really like a small monolith.

What makes this work that that these smaller monoliths need to really be independent. Which means they shouldn't talk synchronously to each other to perform big distributed transactions. They should have their own smaller transactions they can perform.

The larger system operates by putting these smaller Transactions together in larger sagas following that pattern. These should be asynchronous and brokered. Like event driven architectures.
The component systems /mini monoliths should publish integration events.

That's how other monoliths can get information they need to do their transactions. These could be product summaries or currency reference data or whatever. They should get the events and store the data they need.

That's how I model systems built of many smaller autonomous parts (microservices).
I hear a lot of people who break their systems down to too smaller parts.
Transactions are modeled across them in a synchronous fashion.
That's how they get so complex and stay fragile.

IMO Microservices is a mental model that limits the business scope of your monoliths to 1 bounded context. It then gives you a set of principles for making your smaller monoliths interact while staying reliable.

That's why I think there's this back and forth now about monoliths arent so bad. It's right. They get back when they grow beyond a single business area.
Microservices means stopping that from happening. All the other serverless, containers, API gateways, etc are just noise

This was a little stream of my thoughts on it which I'll probably expand to a blog post. Would love to hear other people's thoughts about it.

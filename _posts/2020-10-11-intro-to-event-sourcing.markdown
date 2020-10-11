---
layout: single
title:  "CQRS and Event Sourcing - are they the same?"
date:   2020-07-09 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: As we create software, it is very useful to be able to create diagrams about the software that we want to create, or have created. However this can be time consuming to do, and very time consuming to maintain as the plans/designs change. The solution is to generate diagrams from simpler text descriptions. Lets look at how.
#header:
#teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [tech, architecture, patterns, cqrs, events, event-sourcing]
---

*Command/Query Responsibility Seggregation*, or CQRS for short, is a really common pattern. Even  people who don't know it often find out they have implemented it through necessity without knowing. 

*Event Sourcing* (ES for short) is a MUCH less common pattern that lot of people don't understand or really need.

They are often confused for each other, or confused for being one pattern CQRS/ES.

## So what is CQRS

The name of the pattern gives us two indications about what it is: 
1. Commands and Queries
2. Responsibility Seggregation

### Commands and Queries

Ok, so what are Commands and Queries?

Lets start by defining what it isn't and why we might need it. 


Imagine we have a simple blog application, and you want an CRUD API for the posts. In a *pure* REST based API you would have an endpoint like */api/arcicle/* where you could:
- Create a new post: PUT an Article object at the URL
- Update a post: POST a Article
- Read all the articles: Get */api/arcicle/*
- Read a single article: Get */api/arcicle/{article id}*

So whatever I want to do with the Articles on the blog, I just Get, Put, Post the same kind of object. This is a nice, consistent REST endpoint.

However, there is a sort of inefficiency here, that when I want to create new articles, I have to send all the data about an article, like the CreatedDate (Which should be set on the server), the comments list, the number of views, etc.
All the fields that you might retrieve from the service can be included in the PUT to create, even when that doesnt make sense.
The 'created date' or 'posted by', for example, should be set by the server, not the client. 

To implement these rules we typically change the API and have imperative endpoints, like /api/createarticle
To implement these rules we typically change the API and have imperative endpoints, like */api/createarticle*.
When we 

CQRS is NOT usually implemented with Event Sourcing. However ES is always implemented with CQRS.

At an API level, CQRS can be as simple as have a CreateNewPost structure that's sent to the Write Post API. It has the author, title, tags, and content. Then the ReadPost API returns Post structures that have author, title, tags, body, created date, last edit date, comments, view counts, related posts. (This is useful when the body contains something like markup and you want to generate the html after saving it to turn it into a post.)

These being different models is CQRS. The non CQRS method of doing this would be to have a REST "Post" endpoint that to write you HTTP PUT a full Post model too and GET from to do the ReadPost API.

CQRS can also be interpreted/implemented internally by having a nice normalized data store, and a denormalized cache built up to make reads easier /faster.

Event sourcing is a different pattern that achieves different objectives. It almost always requires CQRS in order to be usable when implemented. In eventaourcing you would still want to break the event logs up into domains and then you need to ensure causal consistency. You don't have to have a message broker though (although I think nowadays most systems would benefit from them)

The key difference about event sourcing is that the log of events is the real store of the data. Tables you update from the events are just like a cache (or materialized view some people call them). They are disposable and should be regeneratable at any time.
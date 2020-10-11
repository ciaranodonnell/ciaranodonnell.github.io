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

However, we have hidden something in this API. 
Creating a new Article is not the same as Updating one, Reading one, or Deleting one. 

When I want to create new articles, I dont want to have to send all the data about an article, like the CreatedDate (Which should be set on the server), the comments list, the number of views, etc.

All the fields that you might retrieve from the service shouldn't have to be included in the PUT to create one.
The 'created date' or 'posted by', for example, should be set by the server, not the client. 

The different between the simple Get structure and the Create or Update structures is the difference between Query and Command. 

Queries are for when we want to read the data back from a system or service, and we typically get all the data. Commands are when we want to modify the data, and we typically *don't* have to provide all the readable data, because we don't have it, or aren't allowed to modify it.

### Responsibility Seggregation

To implement these Commands we typically change the API and have imperative endpoints, like /api/createarticle
To implement these rules we typically change the API and have imperative endpoints, like */api/createarticle*.
Then we create a simple model like a CreateArticleRequest model that only contains the fields the client is allowed to set on the client side and we use that in the PUT. 

Doing this means we have a different Command model and Read model and we have seggregated the logic that performs the associted actions.

So there are 2 reasons people want to use CQRS. The first we already covered, when there are business rules that prevent you from wanted to have the models the same, like the client wants to request a Create by providing core information, and the read model provides much more data, like the Created-Date.

The second good space for using CQRS is when there is work that needs to be done on the inputs to transform them into a read model. 

Lets say for example that the CreateArticleRequest contains the article body in Markdown format, but the read model is HTML so it can easily be displayed in a browser.
This is a good example for the type of seggregation between the Create and Update commands the Read queries.

Often this type of CQRS implementation will actually involve more than one type of storage for this data. 

The simplest will be a cache of the read data. So the database has the body of the article stored in markdown, and when the client reads it, it's taken from the database, converted to HTML and the retrn

The create requests that are sent to the server are stored somewhere, perhaps a database table, and then there is a background process that renders the Markdown in HTML and write the actual Article to the final Article collection.




CQRS is NOT usually implemented with Event Sourcing. However ES is always implemented with CQRS.

At an API level, CQRS can be as simple as have a CreateNewPost structure that's sent to the Write Post API. It has the author, title, tags, and content. Then the ReadPost API returns Post structures that have author, title, tags, body, created date, last edit date, comments, view counts, related posts. (This is useful when the body contains something like markup and you want to generate the html after saving it to turn it into a post.)

These being different models is CQRS. The non CQRS method of doing this would be to have a REST "Post" endpoint that to write you HTTP PUT a full Post model too and GET from to do the ReadPost API.

CQRS can also be interpreted/implemented internally by having a nice normalized data store, and a denormalized cache built up to make reads easier /faster.

Event sourcing is a different pattern that achieves different objectives. It almost always requires CQRS in order to be usable when implemented. In eventaourcing you would still want to break the event logs up into domains and then you need to ensure causal consistency. You don't have to have a message broker though (although I think nowadays most systems would benefit from them)

The key difference about event sourcing is that the log of events is the real store of the data. Tables you update from the events are just like a cache (or materialized view some people call them). They are disposable and should be regeneratable at any time.
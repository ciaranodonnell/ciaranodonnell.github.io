---
layout: single
title:  "CQRS and Event Sourcing - are they the same?"
date:   2020-10-10 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Command Query Responsibility Seggregation, or CQRS for short, is a really common pattern. Even  people who don't know it often find out they have implemented it through necessity without knowing. Event Sourcing (ES for short) is a MUCH less common pattern that lot of people don't understand or really need.
#header:
#teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [tech, architecture, patterns, cqrs, events, event-sourcing]
---

*Command/Query Responsibility Seggregation*, or CQRS for short, is a really common pattern. Even  people who don't know it often find out they have implemented it through necessity without knowing. 

*Event Sourcing* (ES for short) is a MUCH less common pattern that lot of people don't understand well (or really need).

They are often confused for each other, or confused for being one pattern CQRS/ES.

## So what is CQRS

The name of the pattern gives us two indications about what it is: 
1. Commands and Queries
2. Responsibility Seggregation

### Commands and Queries

Ok, so what are Commands and Queries?

Lets start by defining what it isn't and why we might need it. 

Imagine we have a simple blog application, and you want an CRUD API for the articles. In a *pure* REST based API you would have an endpoint like */api/arcicle/* where you could:
- Create a new post: PUT an Article object at the URL
- Update a post: POST a Article
- Read all the articles: Get */api/arcicle/*
- Read a single article: Get */api/arcicle/{article id}*

So whatever I want to do with the Articles on the blog, I just Get, Put, Post the same kind of object. This is a nice, consistent REST endpoint. 

However, this simplicity in the API hides a little complexity.
Creating a new Article is not the same as Updating one, Reading one, or Deleting one. 

When we're creating a new article, we shouldn't have to include all the fields that you might retrieve from the service.
The 'created date' or 'posted by', for example, should be set by the server, not the client. We won't have values for a comments list or number of views.

The different between the simple Get structure and the Create or Update structures is the difference between Query and Command. 

Queries are for when we want to read the data back from a system or service, and we typically get all the data. Commands are when we want to modify the data, and we typically *don't* have to provide all the readable data, because we don't have it, or aren't allowed to modify it.

### Responsibility Segregation

To implement these Commands we typically change the API and have imperative endpoints, like /api/createarticle
To implement these rules, we typically change the API and have imperative endpoints, like */api/createarticle*.
Then we create a simple model like a CreateArticleRequest model that only contains the fields the client is allowed to set on the client side and we use that in the PUT. 

Doing this means we have a different Command model and Read model and we have segregated the logic that performs the associated actions.

So there are 2 reasons people want to use CQRS. The first we already covered, when there are business rules that prevent you from wanted to have the models the same, like the client wants to request a Create by providing core information, and the read model provides much more data, like the Created-Date.

The second good space for using CQRS is when there is work that needs to be done on the inputs to transform them into a read model. 

Lets say for example that the CreateArticleRequest contains the article body in Markdown format, but the read model is HTML so it can easily be displayed in a browser.
This is a good example for the type of segregation between the Create and Update commands the Read queries.

Often this type of CQRS implementation will actually involve more than one type of storage for this data. 

The simplest will be a cache of the read data. So, the database has the body of the article stored in markdown, and when the client reads it, it's taken from the database, converted to HTML and then returned to the client, potentially being added to a cache to improve future read performance of the same articles.

The more involved implementation would mean the create requests that are sent to the server are stored somewhere, perhaps a database table, and then there is a background process that renders the Markdown in HTML and write the actual Article to the final Article collection.
This would enable improved read performance on all articles as you wouldn't need to convert on the fly.

I think the main goal of CQRS is to allow independent logic for the reading and updating of data, primarily to scale the read and write side of a system independently. 

## OK, so what is Event Sourcing?

CQRS is NOT usually implemented with Event Sourcing. However, ES is always implemented with CQRS.

Event sourcing is a different pattern to CQRS, that achieves different objectives.
In Event Sourcing the actual 'system of record' for the data of a system is a log of 'Events' that occurred in the system which mutated state.
So in relation to CQRS, the database of an Event Sourced system is a store of all the successful commands that were applied, stored in the order they were applied.

This is different to other systems, where the database is primarily made up of the result of the commands executed in order. In the blog system examples, a non-event sourced system has a table of Articles in the database and each update command changes the row it acts on. In an Event Sourced system, there table would be ArticleEvents and would have a row for each Create, Update, and Delete command that was received.

Event Sourcing has the great advantage that it means you can:
- Recreate the system state at any point in time but replaying all the commands from inception till that moment. If the commands are structured correctly in the store (with oldValue/newValue pairs) then you could play and rewind the stream from any moment to any other moment. 
- Re-process the events using different logic to create other views of the data, such as calculating changes/minute stats, order most active posters.

The obvious disadvantage to Event Sourcing is that in order to know the state of the system, you have to replay the Events from the beginning. We typically get around this by using the CQRS to create a read summary. 

The most common example people use for Event Sourcing is a bank account.
In any real banking system, a bank account is not stored as a single record in a table with a current balance field.
Instead there is a ledger for the entire bank which has a record of every credit and debit applied to the accounts.

If there is a change to the transaction history, like a failed transaction, the records are removed from that ledger.
It also has a record of 'reversals' which are transactions that undo previous transactions.

If you want to know the balance of any account, at any point, then you add up the credits and deduct the debits.
That is the source of truth for any account.
This also makes it simple to create bank statements for people every month.
Those statements can also serve as checkpoints in the data. We can take the final balance from last one of those statements and just apply the transactions since to get the current balance, greatly reducing the performance implication.

The key difference about event sourcing is that the log of events is the real store of the data. Tables you update from the events are just like a cache (or materialized view some people call them). They are disposable and should be regeneratable at any time.

## So how do these relate to microservices?

Unsurprisingly, they don't directly as They are separate patterns.
Microservices is process for problem solving by breaking down large, complex business landscapes into separate components that each satisfy a sub-domain of the business.

If you want to use CQRS or Event Sourcing in a microservices solution, you need to think of them as the implementation details of each microservice. The event logs that you store should be inside a sub-domain.

If you use CQRS in microservices, or even just communicate between services as events, you can create an event log the same as you do in event sourcing.
This can be used for reporting, trend analysis, replaying later as a source of test data, etc. 
However, if that log isn’t the source of truth then you aren’t event **sourced**, and that’s probably ok.

## Summary

CQRS is a pattern where your system has a separate channel of communication and processing for reading data compared to updating it.

Event Sourcing is a pattern for make the actual source of truth in your system be a sequence of update commands. The current state of the system is determined by processing the events.
You may store that result, but only as a cached value, not as the source of truth.

Both these patterns are useful, but they do solve different challenges.
CQRS is much more common and doesn't need Event Sourcing.
Event Sourcing does normally involve CQRS too.
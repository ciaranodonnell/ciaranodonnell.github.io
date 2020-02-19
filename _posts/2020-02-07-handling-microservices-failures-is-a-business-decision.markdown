---
layout: single
title:  "Handling Failures in Microservices"
date:   2020-01-29 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: This week I was asked the question 2 separate times - How do you handle [partial] failures in microservices? Here's how...
permalink: 
tags: [microservices, architecture, failures, reliability]
---

This week I was asked the question 2 separate times:

> How do you handle failures (or partial failures) in microservices?

That's a good question, and my answer is: *Don't ask me!*

What is mean by that is that **Handling failure is a business concern**.

## Microservices - Designed for Failure

When I present training on microservices, one of the key tenets I talk about is how they should 'own their own up time' and therefore be 'designed for failure'.

There is no way to avoid failure in computer systems, it's inevitable. Therefore to build reliable systems we need to always consider the failure cases: 
- What happens if my dependency can't be found
- What happens if it times out?
  - What if the request times out?
  - What if its the response that times out?
- What happens if one of my two actions fails?
- What happens to everyone else if I fail?
- How do I detect failure?
- How do I recover from failure?

For all the things your application/microservice does, you need to ask these questions of yourself.
These are hard questions to answer. 


## Detecting Failure
The first one you have to solve is "How to I detect failure". This is almost always a technical challenge.
Depending on your communication methods, and your data flow patterns, this can be quite hard or quite easy.

A [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) call can return an error, or it can timeout. The errors can be from the networking layer - the [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) can't be translated or the target machine actively refused the connection. They can come from the application layer, like an [HTTP 500](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_errors) response, or a [400 type](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_errors) response. 

An event that you publish and are expecting a timely responding event for is more challenging. It's not normally a good idea to be waiting indefinitely on a Request/Reply messaging construct. When they're used, there should be an expectation of 
*'temporal decoupling'*, meaning your might have wait a long time for a response. You can then implement timeouts in your sender, you can issue cancelation events, perhaps on a high priority topic construct. 

## Handling Failure

Handling failure is where people really want an answer. However, I don't think there is a single right answer. In fact, anyone who is offering a 'just do this' style answer has probably misunderstood the point of microservices.

Microservices are business oriented services, they map to individual business responsibilities. 
Their requirements are driven by the business processes they are there to support. 
Therefore, the only person who can decide **what** to do when something fails is the Business group you support. 

Let's take some examples:

- In an e-commerce website, if the payment processor service is down, do you say the website is down? Do you allow browsing and basket adding but not ordering? Or do you allow order submission and tell people you'll confirm the order later? Or take the order as if the payment worked and when the payment system comes back try them all and email customers where it failed?

- In the same e-commerce site, if the recommendations service is down, do you hide that part of the page? Or do you put out 5 random products?

-  



## Implementations of Failure Handling

There are obviously any number of ways to implement failure handling. Retry is the simplest, [exponential back-off](https://dzone.com/articles/understanding-retry-pattern-with-exponential-back) is a common addition to that.

The [Circuit Breaker](https://en.wikipedia.org/wiki/Circuit_breaker_design_pattern) pattern is also a common way of remembering that a piece of functionality is broken, down, or struggling so let's not pummel it with more requests.

My favorite way of modelling these things overall, meaning how to we actually continue to operate for our users when things aren't working, is The [Saga pattern](https://microservices.io/patterns/data/saga.html).
Sagas are a great microservices transaction pattern because it:
1. Allows you to build distributed, concurrent, event-based transactions.
2. Gives you a way to handle these technical failures and other real world failures like "lost in the post" or "dropped the last burger on the way to the table". 
 
You just need to model them well.
[Event storming](https://en.wikipedia.org/wiki/Event_storming) / event modeling is a good way to approach that.

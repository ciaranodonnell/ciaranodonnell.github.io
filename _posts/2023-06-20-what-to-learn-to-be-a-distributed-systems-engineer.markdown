---
layout: single
title:  "What do I learn as a Distributed Systems Engineer"
date:   2023-06-15 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: "."
permalink: 
tags: [tech, learning, distributed systems]
--- 

I was asked a question recently by a colleague I used to work with which I have answered quite a few times to quite a few different people. 
The question was:
> I want to pick your brain and get some insights from you, how do you go about upskilling yourself? How do you decide which topics are relevant? How do you go about building side projects?
> 
>My goal is to be of tech lead level by end of year. I would like to build expertise in distributed systems & backend development.

I thought for answering this I would take a leaf out of Scott Hanselman's book and write the answer as a blog post. (I did also answer it directly when it was asked).

## Backend engineering

From a simplistic perspective, backend engineering is just programming business logic.
Most backend APIs just implement the application logic and return simple results.
However, doing this normally requires some deeper knowledge to do a good job.
There are topics like:
-  designing, reading from, and writing to a database.
-  Managing configuration for your services, like the database connection details
-  Calling other APIs
-  Authentication and Authorization, including topics like [OAuth and Open ID Connect](https://auth0.com/resources/ebooks/oauth-openid-connect-professional-guide)
- Design patterns for structuring your logic, e.g. [Vertical Slice Architecture](https://jimmybogard.com/vertical-slice-architecture/), [SOLID principles](https://en.wikipedia.org/wiki/SOLID), [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection), [Unit Testing](https://en.wikipedia.org/wiki/Unit_testing) etc. 
- Team organization practices like Agile, Scrum, Integration testing, VCS patterns like Branching and Merging, and continuous integration and deployment
- Multi-threading, async patterns, concurrency, locking, etc.
- Cloud hosting patterns, PaaS, Containers, etc

These are all good topics to become familiar with to be a backend engineer. 

To be a leader in backend engineering then you should probably be comfortable with each of these ideas enough so that you could train a junior engineer enough to become independent.

### What backend topics are relevant?

*Short answer:* They all are! 

*Long Answer:* I would say the ones that affect you and your team are the ones that will probably be immediately important for you to learn. 
What affects yuou will likely affect other people. 

I would also prioritize the ones you are already a little familiar with. 
If you know a little, but have some gaps, or don't feel like you could really explain it well, then work on that one.

Lots of these problems are interconnected. So the more you explore one of them the more you'll encounter the others.
Encountering the others will give you some high level exposure to them, so they'll fall into the higher priority group.
Then you just work your way through that. 

Which leads us to...

### How to learn? / What side projects?

Learning these topics is really a matter of finding a good explainer, possibly a video or maybe a code along and learning it. For me, I like a video I can code along to, followed by an explainer to how/why something works.
You should chose what works well for your style of learning.

I don't always find a side project to work on these issues.
My preferred options actually is to learn things that will help with my main project/job.
The challenge I find with side projects is that I end up being too ambitious. I try a new platform, with new libraries, new patterns, new everything. Then I struggle to make anything work.
Every new direction I turn gives me a new problem with something I don't recognize.
That makes it hard to learn.

I like to think of knowledge as a web of connected concepts and understandings.
The problem with being over ambitious on a side project is that all the new knowledge is and island, it doesn't connect to the existing knowledge.
That makes it hard for me to really remember it and use it later.

So my advice is to try small examples first, even if they're too small to be really useful on their own.
Then try and use the new learnings or tech **with** stuff you already know.

My example: If learning unit testing, do it with the language and code you already know. Don't swap from Java to Ruby because you heard it's great for unit testing. I wouldn't even start with all the assertion and mocking libraries you can find. Start simple with plain old objects and the basic assertions you get with nUnit or jUnit.


## Distributed Systems

Now this is a different question to backend engineering.
Infact, distributed systems engineering is really an advanced form of backend engineering. Or perhaps it's better to say that someone who is good at distributed systems probably already understands lots of the backend concepts mentioned above.

If you are a backend engineer and you have a front end, or a database, or other APIs you are calling, or multiple services in your own system, perhaps hosted on the cloud, *then you are a distributed systems engineer*. 

Most systems are really distributed systems. 
When you have more then one process in your system, like UI and API, or your code plus a database, then you're a distributed system.

### What topics are relevant?

Really distributed systems problems all stem from 2 problems:

1. When another component (which you're distributed from) fails, you have to detect that and do something without corrupting the system
2. When two (or more) people want to do the same thing at the same time (because that involves more than one part of the system knowing the same thing at the same time)


#### Failure

Handling failure is really the ultimate challenge of distributed systems. In fact there is an often used quote by [Leslie Lamport](https://amturing.acm.org/award_winners/lamport_1205376.cfm) to define distributed systems
> “A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable”

When looking for topics to study on distributed systems you can normally source some good questions by looking at your own architecture diagram. 
Even a basic 2 tier application, UI-over-database, can generate some good questions. 
- What if the database service is down?
  - How do we tell? Error?
  - How do we manage that from the user experience?
  - What do we do about pending writes?
  - (How) Should we cache data locally?
  - How do we check when the database is back? 
- What if the a call to the database times out? 
  - All the questions from the above apply?
  - Does this mean a write failed?
  - Does this mean a write succeeded but we did get the response?

These questions highlight the uncertainty that comes with distributed systems.
These questions basically scale up and replicate out as your distributed system grows. 
- How do I know if something is working?
- How do I keep the user happy when some of it isn't working?
- How do I validate what has successfully happened and what hasn't

There are many strategies for solving these problems, and a lot of them will be returned from a web search for the question.
These will be a great place to start.

#### Concurrency

Something that introduces its own set of questions, plus makes all the above questions about handling failure more complicated, is concurrency.
This is when you have more that one user trying to take actions in the system at the same time. 

The simplest form of this is pretty easy to handle. Two different users editing two different records at the same time. Most database systems can handle this pretty easily.

The core challenge is when you want two people to be able to edit the same record at the same time. A database system can normally actualy handle this with error quite easily, but the challenge is in the user experience, not just the errors.
What if I load a user from a database to my UI, then someone else loads the user, changes it and saves it, then I try to change it and save it. How should this be handled? In fact, how should it be detected even?

The failure questions all become more complicated when there are multiple people editing records.
The detection and handling of failure become more complicated. How to queue up instructions, 

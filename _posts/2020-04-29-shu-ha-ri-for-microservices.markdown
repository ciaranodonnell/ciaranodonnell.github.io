---
layout: single
title:  "Shu Ha Ri for learning Microservices"
date:   2020-03-30 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Shu Ha Ri is a concept in Japanese martial arts that describes the 3 stages of learning. It's been borrowed a few times to discuss how people should learn complex topics. I actually think it applies really well when people are learning microservices too.
#header:
teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [microservices, learning, shuhari, agile]
---

# Shu Ha Ri for learning Microservices

## Journey to Microservices

I have been working with a number of teams at different companies recently and they have each had something in common.
They at or around the start of a journey to microservices.

The thing that these teams have in common is that they have a somewhat incomplete understanding about that that journey entails.

Microservics, as I have written about before, is a problem solving approach to dividing (very) large systems into (business) component parts.
It's not a specific technology that you can buy or adopt, it's not a release process, or a hosting technology (like kubernetes), it's not containers, or serverless, or devops.
It's lots of patterns, practices, and approaches coupled together.
It's like a vision of business aligned, autonomous components, with low coupling, high cohesion, and tesitng and automation thrown in.

Changing the way you structure your teams, break up your components, model and execute your transactions, and maintain and deploy your software is a lot of change.
It takes a lot of time to make all those changes, to habituate the practices, and to believe in the culture.

This is at least equivilant to the level of change that Agile brought to companyes, in fact in some cases this is peoples first time doing agile development too.

When taking teams to agile methodologies, such as scrum, I was a firm believer in the Shu-Ha-Ri method of learning. I think for most teams in enterprise environments this is a good way to learn this microservies culture too.

## What is Shu Ha Ri?

Shu Ha Ri is a Japanese martial arts idea about the 3 stages of learning the art.

From Wikipedia:

- Shu (守) "protect", "obey"—traditional wisdom—learning fundamentals, techniques, heuristics, proverbs
- Ha (破) "detach", "digress"—breaking with tradition—detachment from the illusions of self
- Ri (離) "leave", "separate"—transcendence—there are no techniques or proverbs, all moves are natural, becoming one with spirit alone without clinging to forms; transcending the physical

Aikido master Endō Seishirō shihan stated:

> "It is known that, when we learn or train in something, we pass through the stages of shu, ha, and ri. These stages are explained as follows. In shu, we repeat the forms and discipline ourselves so that our bodies absorb the forms that our forebears created. We remain faithful to these forms with no deviation.
>
> Next, in the stage of ha, once we have disciplined ourselves to acquire the forms and movements, we make innovations. In this process the forms may be broken and discarded.
>
> Finally, in ri, we completely depart from the forms, open the door to creative technique, and arrive in a place where we act in accordance with what our heart/mind desires, unhindered while not overstepping laws."

## What does this mean for tech teams?

So ShuHaRi is a three stage learning process, where the first stage is you just doing the things you're told, without having to understand them, just repeating them to learn how they're done.

In the second step you know processes, how they're done.
You will gain understanding about the benefits of them and will be able to make adaptations based on that understanding.

In the last phase you come to master the art. You can invent your own processes, practices, patterns, etc. Here you're fully absorbed in the culture.

## Applying it to microservices

When adopting microservices, there are so many aspects to the patterns and practices.
It's hard for engineering teams to understand all the different pieces of advice that they hear, as well as to judge whether they're from masters of the art or people still on the journey.

One example of a very frequently abandoned piece of microservcies advice is the '*database per microservice*' idea.
More than half of the teams I have spoken to who are planning to move to microservices have decided that they will drop this idea.

The database per service is really important for the autonomy principle of microservices.
If a service doesnt have it's own database then it will almost certainly need to call other services in real time to get the data it needs to operate, or it will have a shared database that lots of other services also read from.

Autonomy is very useful principle when people are building cloud native applications, especially when they want to enable large scale in terms of user applications, **and** in terms of scaling the velocity of a development organization.

### Autonomy for Reliability

Autonomy allows microservices to be more reliable.
If a microservice has it's own database, then it should be possible to make it function when other services aren't functioning.

### Autonomy for Scale

If services are autonomous and you're expecting them to be able to function without the synchronous involvement of other services, then you should be able to scale them independently.
Keeping autonomy as a princple keeps you thinking this way.

Having a separate database allows us to scale the database as we need, without the rest of the application having to scale the same way, it can also allow us to do things like denormalize, create read models, caches, and other optimizations to scale our services.

### Autonomy for Development Velocity

If services are autonomous, with their own databases, and they dont need other services to run, then you will need to put standard contracts between them.
Following that principle will mean your development teams aren't dependent on each other.
One of the worst development coupling practices is to have a shared database.
It means my service team can't make schema changes without affecting other teams, which overall makes development more complicated, and product velocity slower.

Having seperate databases means that my service team can evolve our product at maximum velocity, as long as we meet our contracts.

### Ripple affect

So abandoning the idea of the 'database per service' means you're avbandoning some (if not all) of your autonomy.
Autonomy causes other good things in microservices, like reliability, scale, development velocity, etc.

Now it's not impossible to share a database while still trying to achieve the rest, but without the understanding of the principles and the practices, and how they interact, it's much harder.

## Shu Ha Ri then

With Shu Ha Ri it's important that you have a 'master' to follow in the early days.
Someone that knows the principles and practices, that can perhaps customize them appropriately at the start if needed.

You then follow this "master's" lead until you understand them yourself, then you can make the right adjustments for you.

The question then is: *"how to achieve this?"*

With Agile people hired agile coaches.
They accepted that expert coaching was essential for getting to grips with this new way of working.
They hired them to staff or they got consultants.
Either way, they imported the expertise and used that person to grow the rest of the team.

I think the same approach is needed here.
Obviously as a consultant who helps people do this for money, you can discount my advice on this.
But the above situation, which I have seen at many companies in many industries, isn't unique.
It's a common example but as I said there are a lot of patterns and practices that make up microservices.
Going through the transformation is not easy and there is no reassonable way to assume developers focused on industry solutions will get it right on their own.
Corporate 'business' teams aren't always very forgiving of IT trying new things and them not going perfectly.

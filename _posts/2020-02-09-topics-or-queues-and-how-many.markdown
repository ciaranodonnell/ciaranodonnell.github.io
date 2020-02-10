---
layout: single
title:  "Queues or Topics, and how many?"
date:   2019-12-23 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Message Brokered systems are increasingly common. What messaging concepts should we be using, Topics or Queues, and should we have a single one or many?
permalink: 
tags: [messaging, events, service bus, eventhubs, event driven architecture, eda, microservices]
---

It's increasingly common for our microservices style systems to be based around Message Brokers and their ability to transfer messages, little packets of data, for us.
Typically message oriented systems have 2 basic ways to model the excahnge of messages:

- Topics and Subscriptions
- Queues

They are similar working and can be used to provide similar models to the other, but they are different.
When we're building systems with these we need to figure out which ones to use. Once we have decided that, we need to figure out if we should be using a single one, a few, or many.

First lets take a look at what the difference is between Queues and Topics, then we can take a look at how best to use them. First, we'll look at Queues as it's useful to understand them when looking at Topics.

## Basic Terms

- **Producer / Publisher** - A process that wants to send a message to one or more consumers
- **Consumer / Subscriber** - a process or application that wants to receive the messages that are sent by Producers
- **Publishing** - the act of sending a message to a Queue or Topic
- **Subscribing** - the act of connecting to a Queue or Topic to receive messages

## What's a Queue?

A Queue is a message brokers implementation of the basic Computer Science data structure called a Queue.
It's a first in, first out structure. It receives messages published to it and adds them to the back of the messages that are already in the queue.
When a consumer connects to the queue, the message broker takes the first message on the queue (the earliest one that was sent to it) and sends it to the Consumer.
When the Consumer has received the mesage, the message broker removes that message from the front of the queue.

### Competing Consumers

Competing Consumers is a situation when two consumers connect to the same queue to receive messages.
In this instance, the message broker will send the first message to the first consumer to connect.
It will then send the second message to the second consumer.
Each time a Consumer gets a new message it will get the next message on the queue.
The message broker will not send the same message to both consumers.
This is useful for when the consumers are doing the same thing with the message.
It's how message brokers easily enable scaling our message consumers.

There is **no way** to have a Queue send the same message to multiple consumers.

## What's a Topic?

A Topic is different from a Queue. Infact a Topic is always paired with the idea of a subscription.
In most message brokers, Topics arent really things at all, they are just an idea. Some, like Azure Service Bus and RabbitMQ, do have Topics as real entities on the mesasge broker.

Topics are something you send a message to.
When a Consumer wants to receive messages that have been published to a **Topic**, they need to create a **Subscription**.

A Subscription is like a Consumers inbound queue.
When a message is published to a Topic, it is copied to each of the subscriptions that have been setup against that topic.

### Competing Consumers

Because Subscriptions are effectively Queues in most brokers, you can have competing consumers on them.
Different consumers can connect to a subscription to receive messages and the semantics will work the same way they do for a Queue. 
The message broker will distribute messages to each consumer and not give the same message to more than one consumer.

Unlike Queues, if you want to have more than one consumer receive **all** the messages that are sent to a topics, they can each have their own subscription.
Each of their subscriptions can have competing consumers too.

This makes Topics and Subscriptions able to do everything a Queue can do, but they can also transparently expand to fan out the messages to other consumers.

## So what do you use?

Well hopefully at this point you're starting to see this answer, but in short - **use Topics and Subscriptions**.

Getting 'always do this' advice from someones blog is generally a bad idea, but in this case you should go with.
Now it's not impossible that there is a good reason in some cases to use Queues over Topics, but in my many years of building brokered systems I havent encountered it. Topics and Subscriptions can do exactly what Queues do - they can take messages from one Producer and queue them up for receipt to a single consumer. They can then expand to do more too, without the original message flow being disrupted. 

If you have a real requirement to only ever deliver messages to one single place, and thats not the absent of a second consumer, but a specific directive that there must only ever be one, then maybe use a Queue. Unless you can enforce that with permissions on the broker, in which case use Topics and remove permissions to make new subscriptions in case the requirements change.

## So how many topics?
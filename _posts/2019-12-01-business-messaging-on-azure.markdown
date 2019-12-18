---
layout: single
title:  "Business Messaging on Azure"
date:   2019-12-03 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Azure has a few different options for Events and Messaging, this is a look at which ones help with which requirements when writing business systems 
#header:
#  teaser: /images/KinesisWithDotNet_DiagramOfKinesis.png
permalink: 
tags: [messaging, events, azure, service bus, eventhubs, eventgrid, event driven architecture, eda]
---

One of the current trends that we are seeing in Business system engineering is a move to Event Driven architectures. 
This architectural style eschews the normal service to sevice relationships of direct synchronous RPC style API calls in favor of pushing out **'Events'** through a **Pub/Sub** middleware.

This change in the relationship is often described as 'Choreography' over 'Orchestration'.

Working at Avanade, most (not quite all) of our Clients are looking to build these systems on Azure using a native service. 
One of the more challenging questions that I'm starting to see is:

**Should I use Service Bus, Event Hubs, Event Grid, or Kafka?**

This is a tough question to answer, and it depends a lot on the requirements of the business problem you're trying to solve. 

## How do we solve it?

I think the most important way to understand this problem, is to get to know a little about the candidate technologies. They have each been created with a specific problem to solve. Therefore they will naturally make a good fit if your problem is like that original problem. They might also be able to help with other problems though as they are quite flexible tools. 

## Type of Broker

I think its good to understand that there are different types of message broker. Fundamentally I think there are really two types, or at least there are in this short list. 

I refer to them as an **AMQP Broker** and a **Distributed Log**

### AMQP Broker

An AMQP broker is the more traditional style message broker, sometimes (formerly?) called Message Oriented Middleware. Like: ActiveMQ, Solace, RabbitMQ, IBM MQ, Tibco, Azure Service Bus, Event Grid, and many, many others.

The below is an image I made for internal training (it was a powerpoint animation) that showed 

![AMQP Broker](../images/2019-12-01-business-messaging-on-azure/amqpbroker.gif)

Although the internal implementations vary, the basic idea is still the same across these brokers. 

1. Different Receivers that want to receive messages create a subscription to them with the broker
2. The broker creates a Queue structure to hold messages to send to the subscribed parties
3. A message sender sends messages to the broker with a Topic (or Subject) for the message.
4. The message is placed into each of the Subscription Queues for the Subscribed receivers
5. When a receiver is connected and ready to receive a message, a copy of it is forwarded to them. 
6. It remains on the queue, but locked, while they process it.
7. Once they have processed it, they send an acknowledgement to the broker and it's removed from their Subscription (forever!)
8. If they dont Acknowledge it, because of disconnect or error, the message will be redelivered to them next time.

A number of these types of brokers speak different protocols. AMQP is a standard protocal, but there is also STOMP, MQTT, HTTP, XMPP, and others. 

These brokers allow you to implement the Publish/Subscribe pattern through Topics and Subscriptions. They also often allow direct 1-to-1 communication with Queues but I won't cover that much as its not a good pattern for business system messaging.

### Distributed Logs

Kafka is probably the most well known Distributed Log type of message broker, invented by employees at LinkedIn. There are lots of good write ups about Kafka and how LinkedIn made it to help them scale. Azure Event Hubs is also a Distributed Log type of broker.

The underlying basic idea of all these types of brokers is the _Commit Log_. This basically is like a big long file that new thinks keep getting written to the end of. The Log is like a Topic in the other kind of broker. You put lots of events that are the same type of thing in a single topic. 

In order to help it scale, all of these brokers actually break the file up into partitions. This lets you group similar events together by source or relevant key, so you can scale the writing and reading, but also maintain some ordering where important.

![Distributed Log Broker](../images/2019-12-01-business-messaging-on-azure/distlogbroker.gif)

With this type of broker, as the message Producer sends the message to the broker, it writes it to the end of the Topic Partition file on disk. It also probably gives a copy to two back broker instances who write a copy too. 
When they Consumer of the message wants to receive it, it can ask for the next message on the Topic but passing the 'offset' (like a message number) that it's read before. 
If it hasn't read before it can start at the beginning. 

Because of this difference, anyone at any point can chose to read from any point in the Topic. Messages arent deleted when they are read, they are left there for a certain amount of time (up to a week on Event Hubs, indefinitely on Kafka).

## Event Hubs


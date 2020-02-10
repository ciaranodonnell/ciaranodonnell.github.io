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
When we're building systems with these, we need to figure out which ones to use. Once we have decided that, we need to figure out if we should be using a single one, a few, or many.

First let’s take a look at what the difference is between Queues and Topics, then we can take a look at how best to use them. First, we'll look at Queues as it's useful to understand them when looking at Topics.

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

Unlike Queues, if you want to have more than one consumer receive **all** the messages that are sent to a topic, they can each have their own subscription.
Each of their subscriptions can have competing consumers too.

This makes Topics and Subscriptions able to do everything a Queue can do, but they can also transparently expand to fan out the messages to other consumers.

## So, what do you use?

Well hopefully at this point you're starting to see this answer, but in short - **use Topics and Subscriptions**.

Getting 'always do this' advice from someones blog is generally a bad idea, but in this case, you should go with it.
Now it's not impossible that there is a good reason in some cases to use Queues over Topics, but in my many years of building brokered systems I havent encountered it. Topics and Subscriptions can do exactly what Queues do - they can take messages from one Producer and queue them up for receipt to a single consumer. They can then expand to do more too, without the original message flow being disrupted. 

If you have a real requirement to only ever deliver messages to one single place, and thats not the absent of a second consumer, but a specific directive that there must only ever be one, then maybe use a Queue. Unless you can enforce that with permissions on the broker, in which case use Topics and remove permissions to make new subscriptions in case the requirements change.

## So how many topics?

I have this conversation with teams that are developing the first message-oriented systems quite frequently.
They normally come up with one of a few basic strategies. 

### Multiplexed Topic

In this design the teams choose to have a single topic, or at least a limit set.
The Consumer then can subscribe to that Topic and check each message it recieves and ignore the ones it isnt interested in.

The benefit to this approach is that is much simpler to configure.
All Publishers and Consumers publish to and consume from the same topic. This means as new message types are added, nothing has to change.

The obvious downside to this is that every consumer has to check the contents of every message to see if its interesting. This can be mitigated with some message brokers that have 'Subscription Filters' or something like that, where the broker is able to filter out the messages for you.

My problems with this approach are:

- The consumers have a lot of extra work to do checking every message, and the brokers have a lot of extra work to do distributing extra messages to every consumer.
- Using a broker specific feature to filter messages is a bad idea. It ties you to specific broker implementation which can change, and you can’t change brokers. It's not the same as the "swap out the database cliche", it's more common that between Clouds, Local machines, and on-premise environments you might use a different broker.
- You can't process messages out of order, even if you want to. This could be back-pressure messages or important Config Update messages. You have no choice but to process messages in order.
- This doesn't work well for other messaging patterns like ["Request/Reply"](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html). All the replies would be queued up behind other messages and could be significantly delayed.
- It can also be a problem if you have data sensitivity issues. If there are some messages that shouldn't be widely distributed, then putting them on a Topic that can be read by every component might be a sub-optimal idea.

### Topic per Producer

This approach is where each system producing messages gets it own topic. Everything they produce they can send to the same topic. If you have well designed service boundaries then this can result in a few good topics that are fairly well defined in terms of what is one them.

In this design the Consumers can decide what Producer services they are interested in subscribing to.
This can solve some of the problems that exist within the Multiplexed Topic.
We can process messages from different producers in separate orders. We can also save a little work for the broker and consumer by reducing the number of unwanted messages.

There are still problems with this approach:

- There are still going to be wasted effort for the Consumers and the broker where they aren't interested in all types of messages. 
- Request/Replies are easier but not properly easy, everyone will get the request from a service, and lots of other consumers could end up with replies.
- If every part of a microservice is publishing on the same topic, then their Domain events and Integration events will be mixed together, exposing domain events to other services. This is a concern if you follow an enterprise Domain Driven Design approach to microservics definition (which you should for complex enterprise systems).
- Consumer applications/services need to know about the producers that create the messages they're interested in. This means if there are multiple Producers for that, it needs to know about all of them. If the producer changes, then it needs to change too. If you want to build a decoupled system, this is sub-optimal.
- Data Sensitivity can still be a concern.

### Topic per Consumer

This is a similar idea to the 'Topic per Producer' approach but you divide the topics up to Consumers. In this case Producers now send to Topics that go to Consumers that they know want the message. Consumers now don't need to worry who sends them the messages.

There are still problems with this approach:

- This is just not how Pub/Sub is supposed to work. The Producer now needs to know about all the Consumers that might want a copy of its messages. If you want to add a consumer, you have to change the producer - this is bad.
- This is effectively the same as having lots of Queues in a system. A Producer now needs to write to each Consumer Topic in turn. It will be very hard to ensure that in the event of a failure, it reliably written to al the Topics. This reliability is the main benefit of using a message broker in the first place.  

These two issues are major issues, you shouldn't take this approach at all, its just not a valid way to work with message brokers. If you dont believe me, ask the person that makes your message broker if its a good idea.

### Topic per Message Type

In this approach you have a different topic for every type of message that gets sent, whoever sends it, and whoever receives it.
Every different type of message you send gets a topic.
Consumers can then pick the specific type of messages that they want to receive.

One of the problems I hear people have with this is that there will be a lot of topics and they feel like thats waste.
However, topics normally arent really a thing that takes up space.
In some brokers, like Solace PubSub+ its really just an attribute of the message as it's sent to the broker.
In Azure Service Bus its something that you can setup to control publish writes and default message attributes, but it doesn't take up space.
It's fair to say that in most, if not all, good brokers Topics are free.

Good points about this approach:

- Consumers dont need to know who publishes messages they consume. So, you don't have to change them when the Producers change.
- Producers dont need to know who consumes messages they send. So, they dont have to change when you want to add or remove consumers.
- Consumers dont get messages that they havent specifically subscribed to.
- Consumers are able to process each different type of message in whichever order they choose. This means important messages like Config updates (great for changing log levels/verbosity for example) can be processed as soon as they arrive.
- Microservices modelled around business domains are able to keep their domain events and Integration events separate.
- Consumers dont have to have extra logic for figuring out what message has been received and how to deserialize it.
- No extra broker features are required to make this work.
- It can be much easier to monitor the subscription queues sizes for these different topics, which better enables you to see where the bottlenecks are.

## Summary / TL;DR

So, to sum up:

- Use Topics and Subscriptions, not Queues. You can make Topics work like Queues but not the other way around.
- Have a different Topic for every type of message. Topics are free and it gives you a lot of benefits for no real downsides.

---
layout: single
title:  "Messaging Brokers as a Single Point of Failure"
date:   2019-12-23 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Microservices architectures are increasingly being implemented with Message Brokers as a central component to facilitate Event Driven Architectures. Doesnt this introduce a Single Point of Failure concern for us? Let's talk about it. 
permalink: 
tags: [messaging, events, azure, service bus, eventhubs, eventgrid, event driven architecture, eda, microservices]
---

As the industry is getting more and more used to the idea that mciroservices are a valuable approach to solving business problems, we are getting more and more conversations about how these services should communicate.

I am a strong proponent of the benefits of Event Driven Microservices. 
While I don't think there is a silver bullet that can solve all problems, I do think this approach is a valuable way of thinking about general business process modelling. 

I have a lot of conversations with people new to the idea of brokered message as a core platform for their systems and one of the questions I get asked a lot is:

_**Does the message broker become a single point of failure for my system?**_

Let's take a look at what a message broker is and what it does for us. Then we can see if this question makes sense to ask, and what the answer is.

## What is a Message Broker

A message broker is a piece of software (or sometimes an appliance) that sits between 2 or more of our services and passes messages (blobs of data) between them. They have a set of features which make them valuable for interprocess communication, namely:

1. They can store messages reliable if the receiver is not able to process them when they're sent. This is called **Message Queueing**
2. They can receive a single message on a topic, and reliably distribute it to more than one recipient who have subscribed to that topic. This is called **Publish/Subscribe**
3. They can deliver a message to a receiver and hold onto it till they confirm they have processed it. If they don't it can re-deliver the message. This is **At Least Once Delivery**

Lots of message brokers have other features and message delivery patterns, but those the core ones that provide the most value to use.

## What's a Single Point of Failure&quest;

When we are building software systems we typically want them to be reliable. We know that machines can fail and software can crash, so we need to make sure that we keep working despite those things happening.
We may need a reboot or to turn on a new machine with a new copy of the software, but we dont want the failure to cause problems in our busienss process, like lost updates.

One of the best ways to ensure that things can continue to work when one thing fails, is redundency, meaning to have more that one them.
If we dont have more than one, we have a single place in our system that can fail, and when it does, it all stops working. That is our single point of failure. 

## What are systems like without message brokers?

Without a message broker we normally build software components that talk directly to each other. Service A needs Service B to do something, so it calls Service B across the network and tells it what it needs.

![Service A calling Service B](/images/2020-01-17-message-broker-as-spof/serviceAcallingServiceB.png)

This is fairly simple. 
However, we said we want to make thing reliable, so we need to have more than one of these services to give us some redundancy.


![2 Service As each calling different Service B](/images/2020-01-17-message-broker-as-spof/MultipleServiceAcallingServiceB.png)

This is better, we now can tolerate the failure of a Single Service A and potentially the other Service B.
But we could do better.
If we make each Service A talk to both Service B instances, then we could tolerate 1 Service A and one Service B failing.
We still have some problems though, plus some new ones:

1. Clients of Service A need to know the address of each instance of Service A.  
2. We need to make sure that Service A knows how to find all the Service B instances.  
3. Our Service Clients need to be able to retry if the service they call doesnt work. THis can lead to SLA challenges if you have to call multiple Service A instances, which each try multuple Service B instances
4. The requests that are being processed during Service A's failure will still fail.

We can solve some of these problems though with other means:

1. We could use a Domain name for Service A and Network Load Balancing.
2. We could use a service registry to allow Service A to find instances of Service B. 
3. We could wrap the whole thing with a reverse proxy that would handle the client rety logic etc.

![Full Direct Call Architecture](../images/2020-01-17-message-broker-as-spof/FullArchForDirect.png)

This is now a more resilient architecture, however, we now have some infrastructure components that are key. DNS, Reverse Proxy and Service Registry. We will need to make sure these things are reliable. We can configure them easily enough and then use NLB and similary techniques to make them reliable too.

However, we still have some challenges with reliability that we will need to solve ourselves:
1. In process requests to a service that fails need to be manually detected and retried by the requestor.
2. What if Service A needs to tell Service B and Service C to do something, how do we make sure both those requests get through?

Fixing these problems will require us to write **a lot of code** to get a half good solution.


## So are we concerned with SPOF?

Any reasonably complex system (more than 3 parts) will need to solve single point of failure problems. The more complex the system the harder that challenge is as everything needs to be made reliable.

As we see above, whether you have a message broker or not, you will need to make sure your critical pieces are reliable.

So the big fear over Message Brokers being a single point of failure doesnt make sense. 

So ignoring the SPOF concern - are message brokers good for us?

## Are messages brokers good for us?

Remember that list of feature of the message broker? Well we should revisit that.
We can use a message broker to solve a lot of the problems we have with complex systems.

1. Service Discovery: We can use the _Pub/Sub_ model with Topics and Subscriptions to prevent the need to know where are services are. They are just on the end of a topic. 
2. Detecting Failures: We can use the At Once Delivery Mechanism to enable a 'Competing Consumer' pattern for our Services. This means that if a service dies while procesing a request, another will get that message from the queue and process it. This happens without you having to write any code.
3. More than one Service?: Again we can use Pub/Sub to provide to let Service A get a message/request to both Service B and Service C with just one operation. The message broker handles reliably distributing to many consumers. In fact, new cosumers can be added without Service A having to know about it. 
4. One of the challenges, or reasons for failure, with services can be handling load. The Queueing feature of the message broker makes that simpler too. We can just consume messages as fast as we can and the broker will spool them for us. If we need more processing power we can increase the number of consumers processing them.

## Making Message Brokers reliable

Each message broker has a different method for becoming reliable. These are normally some form of clustering that makes multiple machines run as a single broker cluster so failure of one doesnt mean total failure. 

However, cloud providers like Azure have Azure Service Bus that takes a huge amount of this burden off you. There is still some work to do when you want to be tolerant of an entire region failing, but for the most part they offer a reliable scalable broker as a service.

## Summary

To summarize, while a lot of people fear message brokers being a single point of failure, the truth is that all complicated systems end up with critical components. You always need to make these reliable. 

So rather than avoid Message Brokers, use them for all their benefits, and take a little time to make yours reliable, or get one as a service.

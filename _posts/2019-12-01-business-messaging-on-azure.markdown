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

## So What do I mean by Business Messaging?

As I said before, a lot of new *cloud native* software that we are building for our customers who want to do *digital transformation* are *Event Driven*.
We can look at the difference between Event Driven and not by thinking about them as seperate programming styles, or even mental models. 

### Imperative Programming 

A more traditional style of thinking about systems programming. Object Oriented languages are imperative. They have commands that change the state of the application as they run. They often run from start to finish taking inputs and running through the program till they're done.

This method of thinking works well for small blocks of functionality. It's very normal for batch style processing, where you have all the inputs and the business logic you've coded does something to that input to generate output.

### Reactive Programming

The Reactive programming paradigm is concerned with data streams and the propagation of change. 
Some things in the system emit events, and others receive and react to them. 
While there are languages to build everything in this style, it's also possible to program like this in our Object Oriented languages. HighlLevel Windows programming is based on events (although there is an imperative message loop underneath). Buttons get Clicked, Textboxes have TextChanged, lots of windows components generate events as the user interacts with them and we program the responses to them.

This model of Events allows us a great degree of reuse. 
We don't have to code a specific button for each of the different actions we want to have happen when a button is clicked. 
The button raises the event and other code can react to it as necessary.

### Reactive Business Systems

So building reactive business systems is one way of solving problems using smaller reusable components. These small components are what we mean by *microservices*. 
While a lot of microservices implementations are not event driven, I think many or them should be.
Let's talk through an example of a system like this

#### Example: 

Let's take a process for onboarding someone to a membership program. This could be something like health insurance, or even a gym membership, or could be adapted to employee onboarding. 

The basic idea: Someone completes a paper form and hands it to an employee. The enter the form into a computer and create an account. The system needs to be able to print a membership card and mail it to them with a welcome letter. 

This is pretty simple to build as a web application. 
There is a form that people fill in, some validation on the form to make sure everything is ok. 
If it is we save the record to our database. 
But we need to make sure we can complete the multistep process of getting a card printed, a welcome letter printed, and envelope printed, and getting them put together and mailed out. 

There are 3 ways to solve this problem.

##### RPC Solution

The first is what I think of as the older, RPC based, orchestrated way. 
This has a function, probably in the Form button click handler that looks something like:

``` csharp
void  OnBoardCustomer(Customer newCustomerData)
{
    SaveCustomerToDatabase(newCustomerData);
    PrintMembershipCard(newCustomerData);
    PrintWelcomeLetter(newCustomerData);
    PrintEnvelope(newCustomerData.Address);
}
```

Each one of the those methods (lets hope its been factored into separate methods) calls a service or API somewhere to accomplish the tasks. If the developer is good, it could be more like:

``` csharp
void  OnBoardCustomer(Customer newCustomerData)
{
    SaveCustomerToDatabase(newCustomerData);
    var membershipCardPrintResult = PrintMembershipCard(newCustomerData);
    SaveCardPrintResult(newCustomerData, membershipCardPrintResult);

    var welcomeLetterPrintResult = PrintWelcomeLetter(newCustomerData);
    SaveCardPrintResult(newCustomerData, welcomeLetterPrintResult);
    
    var envelopePrintResult = PrintEnvelope(newCustomerData.Address);
    SaveEnvelopePrintResult(newCustomerData, envelopePrintResult);
}
```

This is better because in the event that there is a failure in the process we can understand where we got to. 
We might even be able to recover.
However, this method is hard to change because we have hard coded the actions and the order.
If anything changes, we need to come and change this method and re-test the whole process.

##### Event Orchestration Solution

In the Event Orchestration way we follow the same 'my process has these 3 steps, in this order' approach. 
But we can at least make these steps independent and concurrent.

In this example we will publish events on a message broker that describe what we need and subscribe to events that other services will publish that tell us what we need has happened. 
The message broker will give us some resiliancy in terms of handling failures and trying again. 
We'll obviously have to pay for that with idempotency efforts and poison message detection etc, but we wont think about that here as its complex but solvable.

Instead of the function above, we might have a few like this:

``` csharp

void  OnBoardCustomer(Customer newCustomerData)
{
    SaveCustomerToDatabase(newCustomerData);
    
    SubscribeToMessage(typeof(MembershipCardPrintedEvent), OnMembershipCardPrintedEvent);
    SubscribeToMessage(typeof(WelcomeLetterPrintedEvent), OnWelcomeLetterPrintedEvent);
    SubscribeToMessage(typeof(EvenlopePrintedEvent), OnEvenlopePrintedEvent);

    PublishMembershipCardRequestedEvent(newCustomerData);
}

void OnMembershipCardPrintedEvent(MembershipCardPrintedEvent eventData)
{
    SaveCardPrintResult(eventData);
    PublishWelcomeLetterRequestedEvent(eventData.Customer);
}

void OnWelcomeLetterPrintedEvent(WelcomeLetterPrintedEvent eventData)
{
    SaveCardPrintResult(eventData);
    PublishEnvelopeRequested(eventData.Customer.Address);
}

void OnEvenlopePrintedEvent(EvenlopePrintedEvent eventData)
{
    SaveEnvelopePrintResult(eventData);
}

```

This is more complicated now and it still hasn't really helped a lot with the complexity.
What this will have done is made it so we dont have to know what service is doing what we ask. 
We don't need to know even if it is a single service, or another collaboration. 
We blindly request from the rest of the system, and we wait for it to comply. 

We are also able here to 'wiretap' these messages and extend them if we want. 
This could help us with some changes that come in the future. 
The one I have seen most is people want a welcome _Email_ along with the letter. 
This lets people get their confirmation faster.

We can now start a second service, the _Email Service_ that can listen for WelcomeLetterRequested events, and can generate and send off a welcome email. 
We can make that change without changing any of the above code. 
We could change the above, and allow us to capture in the same database that the email was sent. 
But we arent dependent on that change for the email service to start working. 

However, this isnt ideal. Lots of other potential new requirements, like generating website credentials, telling partners required information, perhaps setting up welcome phone calls in a customer care team calendar, these all would require changes to the process.

##### Event Choreography 




## Service Bus


## Event Hubs


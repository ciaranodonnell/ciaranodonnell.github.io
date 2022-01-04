---
layout: single
title:  "Sending a messge on Azure Service Bus in C#"
date:   2021-06-30 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: A quick demo showing how to send a message on Service Bus using C# and .NET
#header:
#teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [tech, architecture, service bus, queues, topics, subscriptions]
---

Message oriented systems are a great way to decouple components in a distributed system.
Azure Service Bus is a good option for people that are building out their applications on Azure and don't want the hassle of running their own broker or working with 3rd party vendors. 

This post shows how simple it is to send messages using Azure Service Bus and the .NET client library.

## Setting up the project

For this article I am going to create a new Console Application that will send the message. 
You will probably have an existing project or application that you will want to add Service Bus publishing too. 
One recommendation, or even warning I would give, is to have the application that connects to your Service Bus be something inside your 'server side' control.
Don't allow client applications like Desktop apps, Mobile apps, or 3rd parties connect to your Service Bus, have a proxy that allows them to call a web API to send a message.
This will allow you to validate the payload, control the serialization format, the topic/queue names, etc.

The Nuget package for the current version of the Azure Service Bus package is [Azure.Messaging.ServiceBus](https://www.nuget.org/packages/Azure.Messaging.ServiceBus/).
We should add this to a project and import its namespace to the Program.cs file

``` csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using Azure.Messaging.ServiceBus;

namespace ConsoleApp1
{
  static class Program
  {
	static async Task Main(string[] args)
	{
      Console.WriteLine("Starting Azure Service Bus Demo");
      
	  // Send stuff to service bus
	}
  }
}
```

## Connecting to Service Bus

So the first this we need to connect to Service Bus is our connection string. 
This can be retrieved from the Azure Portal.

The connection string gives the Endpoint and the Authentication method. 

The Endpoint is easy, its just the URL of your namespace which will be *&lt;yourNamespaceName&gt;.servicebus.windows.net*

The Authentication method can be Shared Access Tokens, or Managed Identity. We can use the default shared access token that comes with Service Bus right now. 

On the left menu for the Service Bus namespace choose the *Shared access policies* option.
That will show you the list of existing policies that are set up on your Service Bus.

![Service bus tokens](/images/servicebus/shared-access-tokens.png)

There will be a default policy there: *RootManageSharedAccessKey*. 

Click that will give you a flyout where you can copy the Connection String from.
This connection string is what you pass to the Service Bus client components.

``` csharp
var connectionString = "Endpoint=sb://ciaransyoutubedemos.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=...=";

ServiceBusClient client = new ServiceBusClient(connectionString);

```

## Sending a Message

Once we have our ```ServiceBusClient``` we can create a [```ServiceBusSender```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebussender?view=azure-dotnet) with the [```CreateSender```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusclient.createsender?view=azure-dotnet) method.

``` csharp

var queueOrTopicName = "myqueuename";

var queueSender = client.CreateSender(queueOrTopicName);
```

Once we have a sender we have options for how we send a message. 

### Single Message

We can send them one at a time by creating a [```ServiceBusMessage```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusmessage?view=azure-dotnet) and passing it to the [SendMessageAsync](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebussender.sendmessageasync?view=azure-dotnet#Azure_Messaging_ServiceBus_ServiceBusSender_SendMessageAsync_Azure_Messaging_ServiceBus_ServiceBusMessage_System_Threading_CancellationToken_)

```csharp
await sender.SendMessageAsync(new ServiceBusMessage("This is a single message that we sent"));
```

### Batch of Messages

The best way to send a batch of messages is to create a [```ServiceBusMessageBatch```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusmessagebatch?view=azure-dotnet). Then you can add [```ServiceBusMessages```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusmessage?view=azure-dotnet) messages to the batch and pass that to an overload of [```SendMessagesAsync```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebussender.sendmessagesasync?view=azure-dotnet#Azure_Messaging_ServiceBus_ServiceBusSender_SendMessagesAsync_Azure_Messaging_ServiceBus_ServiceBusMessageBatch_System_Threading_CancellationToken_)

``` csharp
var batch = await sender.CreateMessageBatchAsync();
for (int x = 0; x < 1000; x++)
{
    batch.TryAddMessage(new ServiceBusMessage($"This is message {x} that we sent"));
}

await sender.SendMessagesAsync(batch);
```

The ```TryAddMessage``` function above is used to add the messages to the batch.
The reason it's a *Try* rather than just an *Add* is because this method checks the size of the batch with the new message in it, and throws an exception if the batch is now too large.  

There is an overload of the ```SendMessagesAsync``` that just takes an ```IEnumerable<ServiceBusMessage>```.
This overload will also error if the batch is too large, but it's match harder to handle at this point as you don't get to find out how many messages you can send.
Therefore it's recommended to use the ```ServiceBusMessageBatch``` as then you find out when the batch reaches it's limit and can handle it immediately.


## More Information

I've been creating a series of videos on building message driven systems and Azure Service Bus. Check it out here:
[Building Message Driven Systems Playlist](https://www.youtube.com/watch?v=57Qr9tk6Uxc&list=PLj1Z4NiDbwIOkkPvM2HFbMMPb9Lr1B_Oj)

The source code for the examples in that series can be found in this Github repository [https://github.com/ciaranodonnell/AzureDemos/tree/master/AzureServiceBus/](https://github.com/ciaranodonnell/AzureDemos/tree/master/AzureServiceBus/)

<iframe width="560" height="315" src="https://www.youtube.com/embed/QgWYeqZdxO8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
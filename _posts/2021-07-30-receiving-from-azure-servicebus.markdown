---
layout: single
title:  "Receiving messages from Azure Service Bus in C#"
date:   2021-07-30 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: A quick demo showing how to receive messages from Service Bus using C# and .NET
#header:
#teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [tech, architecture, service bus, queues, topics, subscriptions]
---

In my last post I showed [how to send a message on Azure Service Bus](2021-06-30-sending-to-azure-servicebus.markdown).
Today I will show how we can receive those messages using that same [Azure.Messaging.ServiceBus](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus?view=azure-dotnet) library.

## Connecting to Service Bus

Same as last time, we need to get our Connection String from the Azure Portal. Then we can create our [ServiceBusClient](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusclient?view=azure-dotnet)

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

      var connectionString = "Endpoint=...;SharedAccessKeyName=...;SharedAccessKey=...";

      ServiceBusClient client = new ServiceBusClient(connectionString);

    }
  }
}
```

## Receiving a Single Message

There may be use cases where you need to just receive the next message from your Queue or Subscription by polling it. 
To receive any messages, we need to create a [ServiceBusReceiver](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusreceiver?view=azure-dotnet).

``` csharp
var receiver = client.CreateReceiver(queueName);

var message = await receiver.ReceiveMessageAsync(TimeSpan.FromSeconds(5));
if (message != null)
{
  Console.WriteLine("Received Single Message: " + message.Body);
  await receiver.CompleteMessageAsync(message);
}
else
{
  Console.WriteLine("Didnt receive a message");
}
```

When we create our ```ServiceBusReceiver``` we can call the method [```ReceiveMessageAsync```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusreceiver.receivemessageasync?view=azure-dotnet#Azure_Messaging_ServiceBus_ServiceBusReceiver_ReceiveMessageAsync_System_Nullable_System_TimeSpan__System_Threading_CancellationToken_).

This allows us to pass a ```TimeSpan``` which says how long to wait for a message before returning.
If there is no message available in the Queue/Subscription before that ```TimeSpan``` elapses then the method returns ```null```.
If there is a message then it will return a [```ServiceBusReceivedMessage```](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusreceivedmessage?view=azure-dotnet) which allows you access information about the message, like its headers, delivery count, etc, and its Body.

## Subscribing to Messages

A more common way to choose to receive messages from a message broker is as a stream.
We can have the Service Bus API call a method in our code for each message that arrives on the Queue/Subscription.

To do this we need to create a [ServiceBusProcessor](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusprocessor?view=azure-dotnet).

``` csharp
var processor = client.CreateProcessor(queueName);
//var processor = client.CreateProcessor(topicName, subscriptionName);
processor.ProcessMessageAsync += Processor_ProcessMessageAsync;
processor.ProcessErrorAsync += Processor_ProcessErrorAsync;
await processor.StartProcessingAsync();
```

To create our ```ServiceBusProcesser``` we call [CreateProcessor](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus.servicebusclient.createprocessor?view=azure-dotnet) and pass it the name of the messaging endpoint you want to process messages from.
If you want to process a Queue then just pass a single string with the Queue name.
If you want to process a Subscription then you pass two strings, the name of the Topic and then the name of the Subscription.

Once you have a ```ServiceBusProcessor``` you need to subscribe to 2 events on it. 
The ```ProcessMessageAsync``` event fires when a message is received from the message broker.

``` csharp
private async static Task Processor_ProcessMessageAsync(ProcessMessageEventArgs arg)
{
  var message = arg.Message;
  Console.WriteLine("Received Processor Message: " + message.Body);
  await arg.CompleteMessageAsync(message);
}
```

In that method you receive ```ProcessMessageEventArgs``` which has a *Message* property that gives you the ```ServiceBusReceivedMessage```.
Once you have processed the message, however your business logic requires you to, you should Complete the message.
The event args parameter has a method for Completing the message, as shown above.

The ```ProcessErrorAsync``` must also have an event handler registered. 
This method is called when there is an error processing the message.
This error could be an exception that bubbled up from your ProcessMessageAsync handler, or it could be an error in the connection to Service Bus.

``` csharp
private static Task Processor_ProcessErrorAsync(ProcessErrorEventArgs arg)
{
  Console.WriteLine(arg.ToString());
  return Task.CompletedTask;
}
```

This handler is primarily for you to log the exception and decide what to do about it. 
You can't process the message here, you only get the exception details.


Once you have registered your event handlers you need to call ```StartProcessingAsync``` on the processor. This is what actually connects it to the endpoint and starts receiving messages. 

When you want to disconnect your processor from Service Bus, there is a matching ```StopProcessingAsync```
## More Information

I've been creating a series of videos on building message driven systems and Azure Service Bus. Check it out here:
[Building Message Driven Systems Playlist](https://www.youtube.com/watch?v=57Qr9tk6Uxc&list=PLj1Z4NiDbwIOkkPvM2HFbMMPb9Lr1B_Oj)

The source code for the examples in that series can be found in this Github repository [https://github.com/ciaranodonnell/AzureDemos/tree/master/AzureServiceBus/](https://github.com/ciaranodonnell/AzureDemos/tree/master/AzureServiceBus/)

<iframe width="560" height="315" src="https://www.youtube.com/embed/QgWYeqZdxO8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
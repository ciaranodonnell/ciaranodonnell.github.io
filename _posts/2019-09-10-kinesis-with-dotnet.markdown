---
layout: post
title:  "Using AWS Kinesis From .NET!"
date:   2019-09-10 13:43:00 -0500
categories: posts
---
One of the beautiful things about .NET Core is that it isn't just a Windows Desktop/Server option. You can write and run C# on just about any platform, including Amazon Web Services.

AWS is a major cloud platform so its quite common that a number of our clients have a presence on AWS, even if their main cloud platform is Microsoft Azure. AWS though, at least in my experience, is a place for Java rather than .NET. Java is the primary language they seem to focus their SDKs towards and when they provide real client libraries they are Java first. None of this is unexpected to me as Java has for years been the primary language in the Server Side Open Source space. (I do hope that balance shifts more to .NET Core now though)

So running .NET on AWS is totally possible even if .NET isn't their primary target audience. That means we need to be able to use the AWS PaaS (Platform as a Service) features in AWS from .NET.

One of the most interesting features of AWS to me, as an Event Driven kinda guy, was Kinesis. It seemed to be their answer to Kafka and Event Hubs. So given my propensity to work with Event Driven, Microservice styled applications I was keen to get a demo up and running with Kinesis and to integrate it into my normal set of libraries I like to use when writing code (my COD Framework).

## Kinesis Client Library (KCL)
Amazon has a Client Library for .NET developers looking to use Kinesis so that was my first port of call. It's on Github (KCL on Github) and there is documentation on the AWS website. So I cracked open the Kinesis documentation and looked for examples of how to send a message on a topic as that's a good first test. You can then find the code to receive that same message as the second test.

## Sending a Message
Unsurprisingly, this simple demo was pretty easy to find.

``` csharp
public async Task SendMessageAsync(TMessage message, string streamName)
{
    //Create the put record request from the AWS client library`
    PutRecordRequest requestRecord = new PutRecordRequest();
    
    //Set the stream you want to send to
    requestRecord.StreamName = streamName;

    //Serialize the data
    requestRecord.Data = new MemoryStream(serializer.SerializeToArray(message));

    //Set a partition key if you need to control ordering
    requestRecord.PartitionKey = partitionKeyFunc(message);

    //Send the message
    await kinesisClient.PutRecordAsync(requestRecord);
}
```

The code above is fairly simple and I got the above code added to my framework pretty simply. I was now able to easily send messages on a Kinesis stream.

## Receiving a Message
Now this is where the information became a little more confusing to understand. I am used to Message Oriented Middleware and have included a bunch of abstractions in my framework. They all typically allow you to open some kind of channel to received messages, often through Events, or (like Kafka) they allow you to call Receive/Poll to get messages. Kinesis was different.

## Kinesis Client Library is NOT a .NET SDK
The surprising lesson I learned quite quickly is that the KCL is not a .NET SDK. Meaning its not just some .NET code that communicates with Kinesis and provides you with a simple API to call to perform actions.

The KCL is actually a skinny SDK wrapper around a Java application, the MultiLangDaemon (or MLD) , that your application needs to start, providing all the information it needs to connect to Kinesis on your behalf. This is unusual for message broker SDKs but lets keep exploring.

The KCL MultiLangDaemon needs some command line arguments, one of which points to a kcl.properties file. This file has a LOT of options about how KCL should manage your connection to Kinesis.

## So how do I receive messages?
So the obvious question if you are starting the KCL code in a separate process is "how do I get messages?". Well you kind of technically don't. Confused? Yeah so was I for a moment.

One of the properties in the KCL.properties file that you pass to the MulitLangDaemon is your 'streamName' and another is your 'executableName'.

Passing the streamName name to the MLD means one thing: you need to start a KCL process for each Stream you want to receive messages from. You will therefore need to have a KCL file for each of those stream subscriptions, which might sound bad but is actually probably a good idea considering the file also has a lot of configuration about how to manage the stream subscription, like where to start, how many messages to receive in a batch etc. You might want to change those for each Stream.

Now, the **executableName** was surprising to me too, but it's also the thing that makes all of this make sense. The MLD process that you start doesn't hand your process back messages, it wants the name of a new process it can start so it can pass messages to that new process.

The MLD starts the executable you tell it to and captures the Standard Input and Output streams of that executable. As the MLD subscribes to the Stream on your behalf it starts to receive messages, it then serializes those to text and writes them to the Input stream of the executable and expects acknowledgements and other commands to be written to the Output stream.

Your job in this executable sub process then is to ensure that when your executbale starts, you tell the KCL SDK that its supposed to be receiving messages. You do this by implementing an IRecordProcessor or IShardRecordProcessor interface and passing an implementation to KCLProcess.Start(recordProcessor) method.

## So How Many Processes?
At a bare minimum: 3

You need your initial process, the one that wanted to subscribe in the first place
The Java MultiLangDaemon that manages your Kinesis connection and spawns your Record Processor executable
Your record processor executable. There is actually one of these per shard so depending on your Kinesis scaling and the number of competing consumers, it could be much more than one.

## So How Does This Look?

![Diagram of Kinesis Setup](/images/KinesisWithDotNet_DiagramOfKinesis.png)

1. Your Application is running on a machine or container with Kinesis running in the cloud
1. Your application wants to start a Subscription to one of the Event Streams in Kinesis
1. It launches the Java Daemon which manages a connection to Kinesis and balancing the leases to shards
1. For each Shard your Application becomes a consumer on, the Java process starts a separate application to consume that Shard
1. As ‘Records’ are received on the Shard, they are serialized by the Java daemon and written to the Std In stream of the processor
1. As the Records are processed, and the processor wants to Checkpoint, those instructions are written to its Std Out stream
1. When a new Subscription is started to another Stream, a new Java daemon and Record processors are started as before.


## What does this mean for me?
The code that handles a message is in a different Process than the one that subscribed, and the code that handles messages from other shards. This means:

1. There needs to be an entry point for that process which allows it to load and initialize a Record Processor
1. Configuration and Dependency Injection needs to run for each shard
1. Singletons won’t be Singletons exactly
1. In Memory Entity Caches need to be built separately for each shard
1. Without simple shared models for state tracking, its likely all this will need a database for storage

It’s a different model of working on message consumption, targeted much more directly at stream processing rather than Event Driven business messaging. This difference from other messaging platforms may be a challenge for the development team.

## Summary
Kinesis is a 'Distributed Log' PaaS offering on AWS and it seems very well suited to either Java clients or to real stream processing use cases. When building Event Driven business systems in .NET, it seems less of an ideal fit. There could be a lot of processes starting in order to make this work, especially with multiple Streams/Topics with multiple Partitions. Everything will be out of process to everything else.

Its a break from the normal mental model and that will give you some challenges. I have actually created a small Open Source library to make this easier to use. I am just cleaning it up before publishing it and will write a new post when I have.

I also think there are improvements AWS could make to enable this to have a LOT less processes for .NET users without breaking the existing implementations. I might document those into a proposal for them in the future.

In the short term, the project I had in mind when I did this research decided to go down the Managed Kafka route on AWS in order to simplify this model to a normal in process one.


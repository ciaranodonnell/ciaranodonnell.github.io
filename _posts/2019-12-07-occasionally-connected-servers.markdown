---
layout: single
title:  "Occasionally Connected Servers"
date:   2019-11-30 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: A common trend I see in consumer facing industries is the need to have local compute serve local experiences while keeping a larger organizational system in sync. While Intelligent Edge is a growing term, I think Occasionally Connected Servers is a more accurate term for when rich experiences, rather than IoT solutions. 
#header:
#  teaser: /images/KinesisWithDotNet_DiagramOfKinesis.png
permalink: 
tags: [architecture, software engineering, retail, consumer, intelligent edge, occasionally connected servers]
---

There are quite a few different industries that have a similar class of problem, requiring a similar class of solution.
The basic summary is that there are different locations where the enterprise meets it's customers and it wants to create richer, more data driven experiences
while catering to the high latency and lower reliability of its connections to it's central infrastructure.

Some examples:

#### Retail

Lots of retail companies are creating their own applications for customer use.
These use to be a mobile app version of their website.

However, that experience is now becoming stale,
and mobile apps are replacing the simple price checker machines that used to be screwed to various columns in a larger store. (Think Kohls in US)

They are become mobile POS apps that you can use to scan things as you put them in your cart to checkout without lining up. (Think Sams Club in US)

#### Food Service

An increasing number of food service locations have some form of Kiosk in their restuarants.
McDonalds and Burger King have these for ordering, while Chilli's has them for table top drink ordering and payment.

In some environments like airport restaurants there are increasinly tablet based ordering systems that allow you to order and pay without speaking to a person.

#### Franchised Agency Industries

Employment agencies, Real Estate agencies, End of life services are examples of types of busines where the work is typically done locally by an office of a larger corporation.
The systems need to be able to run the business locally while reporting everything to the corporate main systems.

#### Common Problems

For each of these types of business there is a local business process that happens at the location, and data that is managed centrally (e.g. Menu) and reported (e.g. Sales).

There is a real desire for the local systems to be able to continue to work while unable to communicated with the central system.
It's not normally considered OK for a restaurant to stop making food or taking orders because it lost its internet connection. 

Lot's of the existing systems in these industries have this capability. I know of several Point of Sale systems that are more than capable of operating for a couple of weeks without internet connection.
They cache the orders they have taken and send them in when they get a connection again.
Storing the menu or product catalog locally.
Restaurant ones will also keep the kitchen displays and receipt printers working fine too.

## So what's new&quest;

I think one of the changes that's happening across these indusctries is the type of experiences that are being created now,
combined with the raised expectations of consumers.

While the POS of sale screens may be able to keep running while the internet is offline, lots of other systems are beginning to have the same requirements.

Some of these experiences now potentially required a lot more information or intelligences to be able to work offline.
In the Employment Agency - it might not be OK to only cache that offices jobs, you might need them for the whole city, or the whole state. Much more data than was synchronized before.

One other thing thats different is that different systems in the same location are now expected to continue to integrate and interact while offline:

- Kiosks need to be able to put orders into the POS while offline.
- Digital signage needs to be able to remove out of stock items from the menu board.
- Customers while still want to be able to scan offers from their mobile devices for rewards and discounts when the internet is out.

## Do we need a new solution&quest;

Again, I will stress that this idea of Occasionally Connected Services is not entire new.
There are systems across the world that already achieve this functionality today, and have done for years.

I do think however, that given the larger datasets, and the more integrations that need to happen locally, we should look at modern ways to achieve this. 

## Modern Approaches

### Event Driven Systems

#### Local Brokers

### Containers at the Edge

### DevOps at Geographical Scale

### Monitoring and Observability

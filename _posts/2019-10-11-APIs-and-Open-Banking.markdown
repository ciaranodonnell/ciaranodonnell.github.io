---
layout: single
title:  "APIs and Open Banking"
date:   2019-09-10 13:43:00 -0500
excerpt: Open Banking is a hot topic, especially in the banking industry at the moment. However I think its actually a trend that will have a broad range of effects across a number of consumer facing industries.
categories: [posts]
tags: [banking, apis, openbanking, innovation, disruption]
---

Open Banking is a hot topic, especially in the banking industry at the moment. However I think its actually a trend that will have a broad range of effects across a number of consumer facing industries.

## What is Open Banking
Open Banking is a drive around the world to really open up the banking industry in order to give consumers more control over their data as well as to drive better experiences and more value for them overall.

One of the major drivers for Open Banking is the European regulations that known as PSD2 (Second Payment Services Directive). This regulation forces banks to open up access to account data and actions through APIs.

Where banks in the past have effectively had a monopoly on providing financial services to their customers, this regulation is really opening up the market to enable other 3rd parties to create experiences that access your banking data (with your permission of course) to provide some value over the top

## What are these new experiences?
The two main types of services that are enabled by the regulation are:

### Account Information Services
These are services like mint.com or YNAB which are able to read account information from your bank accounts and provide you with an aggregated view of that information. Mint.com is a well known instance of this in the United States. It allows you to pull in information from all your accounts, checking, savings, mortgage, credit cards, investments etc. You can then get recommendations about other financial products that might suite you, or plan budgets and debt repayment programs. These are the value additions that people want. They are also only enabled by a service like this and Open APIs. No financial provider on their own would be able to do this for you.

### Payment Initiation Services
Zelle is a popular example of this type of service in the United States. Payment Initiation services allow you to make digital payments between yourself and a 3rd party, often a retailer or a friend that you want to send money to.

## Open &lt;Industry&gt;
One of the differences between Open Banking and other incremental changes in technology is that this change is driven by regulation, not in every country but in many.

The expectation that has been codified in law now is:

1. The Consumer is in Control
1. They own their data
1. They can share it with whoever they want
1. These integrations are free

On top of those, there are de facto standards too:

1. These integrations are near real time
1. There should be value add from the integrations
1. If I don’t like the service, I can leave

The fact that these are codified makes them different. I think it will have a broader effect on how people view their interactions with any company that they provide data to and buy services from.

It will drive innovation and disruption across all industries as people take these expectations with them to Tel Cos, Insurance, retail, healthcare etc.

## And the Challenge
The growing problem I think this will bring to any industry is that creating Open APIs for broad consumption presents a technical challenge. Many industries are typified by companies having large, expensive legacy applications that house the data they would want to expose through APIs.

How can you create a real-time integration API, with broad reach and any-time availability on top of a system that has fixed capacity, maintenance windows, and potentially (probably) poor integration options too.

In my day job I meet a lot of clients that are meeting this challenge head on and we help find solutions to these problems. Often we enable clients to not just deliver something new on top of their legacy, but using Digital Decoupling, Strangler pattern, event driven architectures etc, we're able to give people a route away from their legacy systems.
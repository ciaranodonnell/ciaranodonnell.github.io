---
layout: single
title:  "Distributed Transactions are trouble. Use sagas"
date:   2020-05-10 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: Building distributed systems almost always entails making changes to data across the system. Traditionally we have used distributed transactions patterns to do this, but they are terrible to operate. We need to model distributed transactions without these patterns.
#header:
#teaser: /images/Gartner_Hype_Cycle.svg
permalink: 
tags: [microservices, distributed transactions, sagas, distributed systems, transactions]
---

## Distributed Transactions are Trouble


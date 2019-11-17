---
layout: single
title:  "Keeping Documentation in sync with Azure Devops"
date:   2019-11-19 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: One of the real challenges as we write complex software is documenting it well, and keeping that documentation up to date. Here I show a technique I use for managing the process of documentation updates and making them accessible using Azure Devops
#header:
#  teaser: /images/KinesisWithDotNet_DiagramOfKinesis.png
permalink: 
tags: [documentation, azure devops, azdo, git, agile]
---

There is a debate I see a lot in software engineering circles about whether code should be self documenting or not. This is normally argued by people that don't want to spend the time writing good comments or updating documentation. I think these people lack a level of empathy for their future team members, including their future selves, and I can't help but think that they hvaen't been on a complex project for long enough to have to support it over time. 

As you can probably tell, I don't think complex software **can** be self documenting through the code. The code bases become too large and often they have complex bits in them. I don't just mean sections of high cyclomatic complexity, but parts where it can be hard to understand the business requirements and the value delivered by the code. 

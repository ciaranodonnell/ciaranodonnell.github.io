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

As you can probably tell, I don't think complex software **can** be self documenting through the code. The codebases become too large and they often they have real complexity in them.

Dont get me wrong thought, I think trying to write simple code that is easy to understand just by looking at it is a good idea.
Keeping Loops short enough so you can see the open and close braces at the same time, breaking things out to clearly named functions, and making our variable names explicit, these are all good ideas and we should do them.
However, that isn't enough to make entire enterprise applications devoid of comments and documentation.
There are other types of documentation, such as:

1. High level design documentations
2. Business Requirements documentation
3. API or Interface documentation
4. Events and Command message contracts and who publishes and subscribes to them.
5. Configuration values, their types and their effects

Just about every large project I have been involved in has had a need for this documentaiton. Most of them have a series of word documents that get created fairly early in the process. Then they struggle to keep them up to date through the project.

Other projects havemore success by created a wiki for the project. This normally works better than a Word document as it allows linking between the sections and a web like structure that better matches the interdependency between the components in a large system.

## Documentation as Code

The major issue with both of these is keepign them up to date as the project moves forward. Making sure the documentation accurately reflects the system. This becomes harder as the code is worked on in branches and by distributed teams.

I think the best solution for keeping the documentation and application code in sync is to treat them the exact same way. I really want to be able to use the same tools to edit the documentation as you do code. The documentation and code should live together, be edited in the same tools, managed in the same source control tools, and branch and merge along with the code.

While I am a big fan of GitHub, I typically use Azure Devops for my work code repository. Therefore this example is designed around making this work in AzDo. 

## Setting Up and Azure DevOps Project

Open Azure Devops and login to your account. If you don't have an account you should create a free one.

![New Project Button](../images/2019-11-19-keeping-documentation-in-sync-with-azure-devops/create-new-project-button.png)

Enter the project information as you want:

![New Project Button](../images/2019-11-19-keeping-documentation-in-sync-with-azure-devops/new-project-form.png)


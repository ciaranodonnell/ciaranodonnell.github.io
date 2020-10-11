---
layout: single
title:  "Making Diagrams from Text"
date:   2020-07-09 13:43:00 -0500
categories: posts
comments: true
toc: true
author_profile: true
excerpt: As we create software, it is very useful to be able to create diagrams about the software that we want to create, or have created. However this can be time consuming to do, and very time consuming to maintain as the plans/designs change. The solution is to generate diagrams from simpler text descriptions. Lets look at how.
#header:
#teaser: /images/shuhari/ShuHaRi.png
permalink: 
tags: [tech, architecture, design, diagramming]
---

As we create software, it is very useful to be able to create diagrams about the software that we want to create, or have created. However this can be time consuming to do, and very time consuming to maintain as the plans/designs change. The solution is to generate diagrams from simpler text descriptions. 

To try and get around these downsides, I want to show a few ways we can get our diagrams generated for us from a simple text based language. 
Creating diagrams from text gives a few benefits:

1. It's simple to create diagrams this way. Rather that creating a large XML document, the approaches I'll show will have simple languages for describing diagrams.
2. It's simple to store. We can create the diagrams if we want to, but our primary storage can be the text that generates them. This can be commited to our version contol system too.
3. It's easier to maintain. Putting the text in source control means we can track the real changes to the diagram over time, and merge changes to diagrams between branches. 

So what our the options?

## Web Sequence Diagrams ([websequencediagrams.com](https://www.websequencediagrams.com))

Now, as the URL suggests, web sequence diagrams only does one type of diagram - sequence diagrams.

Now that might sound like a big limitation, but sequence diagrams are actually pretty useful for describing the behavior of a system. 


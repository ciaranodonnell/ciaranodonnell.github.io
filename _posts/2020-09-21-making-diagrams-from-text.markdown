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

Entering some simple test like this:

![Websquence Diagram Screenshot of text](../images/2020-09-21-making-diagrams/websequencediagrams-text.png)

gets you a diagram like this:

 ![WebsquenceDiagrams.com Screenshot of Diagram](../images/2020-09-21-making-diagrams/websequencediagrams-image.png)


That's pretty cool, and we can end up creating some pretty big diagrams:


 ![WebsquenceDiagrams.com Screenshot of Diagram](../images/2020-09-21-making-diagrams/websequencediagrams-bigimage.png)


The last thing worth point about about [websequencediagrams.com](https://www.websequencediagrams.com) is it can produce diagrams with different styles.
Some of these styles are part of the paid version of the site, but its good to have options.

The paid version ($15/month at time of writing) has features like saving your work, larger diagrams, more styles, and 'includes' of other sub-diagrams.


## Mermaid.js ([https://mermaid-js.github.io/mermaid/](https://mermaid-js.github.io/mermaid/#/))

Mermaid.js is good because it's Javascript based.
This means that, using nodejs, you can run it as a local image generator on your machine, without a website required.

You can also use it on your website in a browser too. 
In fact, there is a live editor for mermaid JS diagrams at [https://mermaid-js.github.io/mermaid-live-editor/#/edit/](https://mermaid-js.github.io/mermaid-live-editor/#/edit/).


## yUML

This is cool


## Diagrams as Code ([https://diagrams.mingrammer.com/](https://diagrams.mingrammer.com/))

This is cool 

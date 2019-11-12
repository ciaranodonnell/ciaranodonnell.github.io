---
layout: single
title:  "Switching my blog to Github Pages"
permalink: 
date:   2019-11-11   19:43:00 -0500
excerpt: Over the last few months I have tried out a number of different blogging options from self hosting to hosted services. Recently I found out about github.io and decided to give it a try. This is how I did that
categories: [posts]
comments: true
toc: true
tags: [github, blogging, hosting, dns, social]
---

Over the last few months I have tried out a number of different blogging options from self hosting to hosted services. 
First let me tell you a little bit about those and then I will talk about moving to Github.io for hosting.

## My old blog
A long time ago in a galaxy far far away I had a blog. Its where I got the title 'wanna be developer' from, which is a 
moniker I've used a lot since then. The first post on that blog was about wanting to be one of the great 'teacher' 
developers that I looked up to at the time. 
It was a wordpress.com hosted blog at  
[http://wannabedeveloper.wordpress.com](http://wannabedeveloper.wordpress.com). 
The last post on that platform was during Build 2012 when Windows 8 launched. 

In the interim i have made other attempts to get into the community. However, I have always had the same kind of 
problems keeping it going. The first and most prominent of those what that I didn't think I had a useful contribution 
to make. There were peeople going on .NET Rocks and talking about big frameworks and I felt that my voice wasn't 
going to be useful in the same crowd as those. Working at a consulting firm meant the cool stuff I did was 
proprietary and secret so it would have just been opinions. 

## Doubling down on social
About a year ago my role changed and I took a leadership role for Avanade in North America. I am now the leader 
of our Software Architecture Center of Excellence. As a group we are charged with helping sales (as all
 groups in consulting firms normally are) but, perhaps more importantly, 'Technical Enablement'. 
 That means that while an important part of my role is solving customer problems, it is also to do training and 
 create recommendations for people across the company. 

Over the last year I have been creating this training and enablement content and, at first, it was maybe my least 
favorite activity in my role. I felt the same pressure I had felt for years, that it was crazy that I would have 
something to teach all of Avanade's 20,000+ consultants. However I have come to learn an important lesson:

**Everyone has something to contribute!** 

I've had the benefit of working on a number of unique projects over my career with some very challenging requirements. 
I may not have built big frameworks that everybody used but I have built some interesting systems 
and learned a lot of things along the way. That experience is unique. 
The learnings are valuable and the perspectives I have can be useful to other people. 
Not everything I create will be gold, and some of it might be a little wrong, 
but by creating it I am taking part in the conversation.
Scott said in his [Being a social developer](https://myignite.techcommunity.microsoft.com/sessions/84134?source=sessions) 
talk from Ingite 2019 last week something along the lines of "dont be scared to be wrong. Being wrong means you get two 
blog posts for one, the original one and the one where you admit to being wrong and explaining your learning".

This is the way I am approaching everything now. I hope I am right in the stuff I put out, but I am willing to be wrong,
to accept that I dont know everything, and I want to learn, even if that happens in public. 
I went to Ignite last week, and I have been to a few Build conferences before, and each time I do I realize the enormous
amount of things I don't know. I turning that thought into opportunity instead of impedement now.

So enough of my rambling about this crap, let's talk tech:

## What blog engines have I tried?

1. [Hosted Wordpress](www.wordpress.com)
2. [Self Hosted Wordpress](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/WordPress.WordPress)
3. [DotNetNuke](https://www.dnnsoftware.com/)
4. [BlogEngine.Net](http://www.dotnetblogengine.net/)
5. [Ghost](https://ghost.org/) ([self hosted on azure](https://adamtheautomator.com/running-ghost-on-azure/))
6. [Blogger](https://www.blogger.com/)
7. [Github Pages](github.io)

Why did I try so many? I think I was looking for a set of specific requirements which I neglected to think about first. 
I didn't know what other bloggers experiences were, or what technology they were using, but I wanted something to be easy.
I think I was looking for something that:

1. Looked kinda cool and professional
2. Was easy to create content for
3. Could have posts, pages, comments, and analytics
4. I could keep the content accessible to myself
5. Had a good editing experience
6. Was extensible: I could build on top of or had plugins for other cool features
7. Was cheap or better yet free

## Hosted WordPress

This is the first thing I tried and the TL;DR is that it's pretty good. 
In fact my old website is still up there, 
although i assume it gets no traffic (I hope not as the content is old and I'm likely not that proud of it).

Wordpress has a LOT of themes that you can chose from in order to make your site look professional. 
You can chose from a whole selection of free or paid ones in the wordpress UI or you can buy them
 in other places and upload them into your site. This enables Wordpress to definately look profressional. 

 It also has a pretty simple editing feature where you can add paragraphs, images, etc, as well as edit the 
 meta data for the post such as tags and categories

![Wordpress editor](../images/29-11-11-WordPress-hosted-editor.png)

Wordpress supports posts, pages, and other content types. It allows the embedding of videos and other 
content and it works well. This is a pretty full featured product and there are a bunch of plugins that can
help extend the functionality. However you can't edit stuff yourself well (or at least you couldn't when I used it) in the 
hosted version. You need to self host in order to be able to change the source. 

In terms of the *content accessibility* wordpress also does well. It allows the importing and exporting of your content to files.
This allows you to move content between sites, as I did when I switched from hosted to personal wordpress site. 

Finally, in terms of cost, Wordpress can be free, and free is normally pretty cheap. The cost of free here is ads. Wordpress
will add advertisements to your site and monetize them. You won't see any of that money, it covers the cost of hosting.
For some people this is fine, for others you may make the choice that this isnt for your, especially if you are trying
to monetize your web property. If you want to get rid of ads and get access to more features in the hosting you can pay 
for it. I never did this so I cant comment on what that experience is like. I always valued free, possibly because of my
sporadic commitment to it, plus the fact that sometimes during this journey since 2006 when I started were harder than others.


## Self Hosted Wordpress

This is pretty much the sames as the Hosted version but you are doing the hosting. I have only tried this one way - Azure PaaS. 
The WordPress official template from the Azure marketplace, and then converting my content over from the hosted via the 
export and import functionality. 

This did work but it isnt as easy as I wanted it to be for adding plugins and stuff like that. WordPress is pretty good 
about updating itself and doing plugin download and installation, however it didnt feel perfect to me. This could be my
own lack of familiarity with the product or it being on Azure PaaS and me not being able to see it doing things on a 
file system. 

I didn't run this that long as I was running it on the free tier of Azure, which meant the performance was kinda sucky for me
due to the low capacity and the fact that my under performing website was ALWAYS code booting. 

I wouldn't recommend this unless you're a wordpress developer who wants to customize their install. Although if thats
you - what they hell are you reading this for? Go do it.

## Dot Net Nuke

DNN is a Content Management System (CMS) which I remember from the pretty early days of .Net. I remember it 
being quite popular a long time ago but I am not as connected to that space and this could be dead by now. 

I used this also on Azure PaaS from the marketplace and I ran it for a little while before giving up. 

DNN is now part of a larger platform play called Evoq which I haven't looked at. It's the free, feature limited SKU and 
therefore it's probably fair that its not as good. 

I stopped using it pretty quickly as I wasnt able to get pages and posts and stuff working how I wanted them. It 
wasn't as easy as I wanted it to be and so I gave up.

## BlogEngine.Net

I ran this on Azure PaaS too and I kinda liked it. I got it working and got its basic theme configured so I had
a website that I thought looked pretty ok. However it didnt have (or I didnt work out) the level of customizability, 
theming and add-ins that I wanted. 

This blog site didn't get a lot of love from me over the short period I was hosting it. I wasn't as easy as I'd hoped and 
was having the cold start performance issue. Plus it was a Web App and Database hosting in Azure and that costs some 
noticable money for a hobby for anything about the basic tier. 

## Ghost On Azure

While Looking for alternatives for BlogEngine I found Ghost. I hadn't ever heard of it before but there was a cool
blog post on [how to run Ghost on Azure](https://adamtheautomator.com/running-ghost-on-azure/). So I follow that 
and it worked. I had Ghost up and running. That blog also showed me how I could make Ghost more responsive by turning 
it to a production server and making it 'always on', so it didn't go to sleep when people werent accessing the site. 
This solved the performance but did mean the burn rate was going up. 

When I was converting from BlogEngine.NET to Ghost I learnt a good lesson about why self hosting is hard/annoying. I 
accidently deleted by database for the BlogEngine.NET blog. That meant that the posts I had added to it were gone. 

This reminded me that one of hte important things I was looking for was a way to have my content be reliable, even through
the transiency of my blogging platforms. 

To be honest, I really quite liked Ghost. Out of all the self hosting options I used I felt like this was my favorite. I 
thought the theme it has was nice, the way the editor worked was nice. It had autosave for drafts, the option to insert
content like text or markdown. It just had a nice simple UI and simple features. 

But I didnt stop with Ghost. Having it run a production server was breaking my cheap/free requirement and I wanted 
to find something better than that. So I kept looking around. 

## Scott Hanselman made me buy a domain name

Ok, so that's not really true. More like Scott Hanselman inspired me to buy a domain name. Actually its more like Scott 
Hanselman inspired me, so went and bought a domain name as a way to make it more 'official' that I was a blogger. 

Anyway, point is, I bought a domain name. [ciaranodonnell.dev](ciaranodonnell.dev) to be exact. Hopefully, that's 
the domain you have used to get to this content. 

I bought the domain through [Google Domains](https://domains.google/). They were the cheapest for a .dev domain so 
I went with them. I admit to only doing about 2 or 3 minuts of price comparison, but they were quite cheap so I bought it. 
I actually bought it while Scott was still talking away on stage. It went through really quickly and had a really easy 
domain management page to point it at my Azure Ghost blog. So I did that.

## Back to Ghost on Azure

Now I had a domain pointing at my blog, and for about 30 seconds everything seemed cool. Then Azure or Google or someone 
in between started to redirect my http traffic to https. Well that's cool, because https is secure and people like to 
use it. However, it meant I would have needed to buy an SSL certificate. They aren't free. Not to buy your own anyway. 

I couldnt seem to find a way to make Azure or Google or whoever stop doing this. Which meant my domain name was now 
not usable as browsers don't let friends browse https with crappy certs.

## 5 minutes on Blogger
Not 5 minutes of reading time for you. I mean I spent 5 minutes with a blog on Blogger. I tried this route as
Google owns Blogger, and therefore Google Domains is like "hey, wanna use blogger for that domain?". So I tried it. 
And I found out pretty quickly, blogger will allocate an SSL cert so it all works nice in browsers. 

Awesome!

So I continue the setup. Wait a minutes, whats this? In setting up my profile Blogger is asking if I want to list my AOL 
or Yahoo instant messenger screennames. Oh dear! This implies that Blogger is not an up to date, well maintained platform.
It also doesnt have a good theming system that gives anywhere near the results of wordpress or ghost. 

Abort!

I remove the association to the domain. Blogger is a no go.

## Github Pages / Github.io

I had heard about Github pages before. Kinda. I'd seen a website before that said it was a github.io website, but I 
didnt know what that actually meant.

So I took a look at [github.io](github.io) and I think you should too. 

Long story short:
1. Github is a cool place to save and version files
2. Github pages allows you to store files that could be a static website
3. Static websites are sooooo 1993
4. Github Pages allows you to use Jekyll to generate a static site
5. Jekyll is a Ruby based tool that means you can write blog posts in markdown and have some yaml config files and it will magic up a static site for you
6. Github Pages actually does the Jekyll stuff for you on a commit to your Github pages repository (see 1 and 2)
7. Jekyll has good theming support (once you get it working)
8. Github Pages supports custom domains and issues SSL certificates for them
9. Github Pages is free for normal people doing normal things. 

Short Story Long:
I have a github account called [ciaranodonnell](https://www.github.com/ciaranodonnell). Therefore to use Github Pages I can 
create a public repository called [ciaranodonnell.github.io](https://www.github.com/ciaranodonnell/ciaranodonnell.github.io).
I cloned this repository to my laptop and was able to use [Jekyll](https://jekyllrb.com/docs/) to create an empty/template site.

I followed the process to install Jekyll for Windows and it worked at first. I was able to create the template site 
and it seemed cool. I could even run the local webserver tool to allow me to test changes to my site locally. 
However, all wasn't perfect for long. When I started trying out various themes I started to get errors that they weren't
compatible with the version of Jekyll that was compatible with Windows. I order to solve this I installed Jekyll in 
Ubuntu on Windows. I am on a Windows Insider build so I have WSL2 but that just hit the slow ring today. 
I needed to *sudo apt install* some things first, namely: gcc, gc++, make. perhaps some others. This worked well 
for me and gave me an excuse to use WSL. I might go back and explore the Windows installation option and make sure that
works. 

In order to edit my site I am using visual studio code to edit markdown files. When I want to commit them to the blog I 
commit them to master and push them to github. Within about 20 seconds my site is rebuilt and updated. 

![Visual Studio Code editing Jekyll site](../images/29-11-11-vscode-editor-jekyll.png)

I still have a lot to learn about Jekyll. It took me far longer than it should have to apply a theme and get it working. 
This isnt helped by the fact that when it fails github renders empty html pages. 

I am trying right now to get comments and google analytics enabled and working. I have them setup in the respective 
services but I need to get the yaml config files correct in order for jekyll to render them. Perhaps I am missing an
entry in my Gem file (a concept I still have to explore).




## Summary

So after a a long time without really using a blog I went on a whistle stop tour of blogging software before landing on 
github pages. Thats what I'm using now, and so far, it's basically working. 

I am very interested in other peoples experiences here. This is something I have spent a lot of time on and I think 
I have a long term solution to. If you have used github pages feel free to let me know your experience. 




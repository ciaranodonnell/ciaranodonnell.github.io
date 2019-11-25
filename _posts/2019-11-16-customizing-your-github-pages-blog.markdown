---
layout: single
title:  "Customizing Your Github Pages Blog"
permalink: 
date:   2019-11-16   19:43:00 -0500
excerpt: My last post was to get you setup with a github pages blog, this one will help you customize it so you can start blogging with it. 
categories: [posts]
comments: true
toc: true
tags: [github, blogging, themes, social, jekyll]
---

## Getting tools Setup

I use [Visual Studio Code](https://code.visualstudio.com/) for editing my blog. It allows me to open the blog folder, edit the files, use an extension for MarkDown authoring, and has cool Git integration. 
- The MarkDown extension I use is [Markdown All In One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
- I use [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) for getting source control information in VSCode
- On one of my machines I use [Markdown Preview Github Styling](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-preview-github-styles). This tool makes the preview look like Github Markdown.

Once you have Visual Studio Code installed and configured with the extensions, open the folder for the jekyll site. 

## Naming your blog

So it's great that you have the domain name from GitHub and the blog is recognizable. 
What we need to do first is setup out Jekyll site so it has the right name etc for out blog. 

In order to make this make sense as we go through it, I'm going to assume your name is **Felicity Smoak**. 
So we'll call your blog **Felicty Smoaks Blog** and put it at **TeamArrow.github.io**.

The basic attibutes of you site are stored in the *_config.yml* file. Open this file and it should look like this:

``` yml
# Welcome to Jekyll!
#


title: Your awesome title
email: your-email@example.com
description: >- # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb
github_username:  jekyll

# Build settings
theme: minima
plugins:
  - jekyll-feed

```

I have removed a bunch of the comment lines (lines that start with #) from the above. 

In this file you can change some of the basic things:
- **title**: This is the big title at the top of the site. We can set this to *"Felicity Smoak's Blog"*
- **email**: This is the email address that will show up on the site. We'll set it to  *"felicity@teamarrow.com"*
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

Remember from the previous post on using Jekyll, you can preview the site locally by opening a Terminal/Console/Ubuntu window, navigating to the folder with your site in it, and running: ``` bundle exec jekyll serve ```. 
Then in your browser navigating to [http://localhost:4000](http://localhost:4000)

## Naming your blog

So it's great that you have the domain name from GitHub and the blog is recognizable. 
What we need to do first is setup out Jekyll site so it has the right name etc for out blog. 

In order to make this make sense as we go through it, I'm going to assume your name is **Felicity Smoak**. 
So we'll call your blog **Felicty Smoaks Blog** and put it at **TeamArrow.github.io**.

The basic attibutes of you site are stored in the *_config.yml* file. Open this file and it should look like this:

``` yml
# Welcome to Jekyll!
#


title: Felicity Smoak's Blog
email: felicity@teamarrow.com
description: >- # this means to ignore newlines until "baseurl:"
  Just a blonde girl from Starling City doing crazy things with computers because I'm a hero and I'm in love with Oliver Queen.
baseurl: "" # the subpath of your site, e.g. /blog
url: "https://teamarrow,github.io" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: felicitysmoak
github_username:  teamarrow

# Build settings
theme: minima
plugins:
  - jekyll-feed

```

I have removed a bunch of the comment lines (lines that start with #) from the above. 

In this file you can change some of the basic things:
- **title**: This is the big title at the top of the site. We can set this to *"Felicity Smoak's Blog"*
- **email**: This is the email address that will show up on the site. We'll set it to  *"felicity@teamarrow.com"*. In the default template this appears in the page footer.
- **description**: You can put a basic description of the site. In the default template this appears in the page footer.
- **baseurl**: If you have put your blog in a folder, like 'blog' inside the repository, then you need to put that there as it will need to be reflected in the links too. If you followed my guide then you haven't and this should be empty.
- **url**: This is the Url you in github pages. For this example its *http://teamArrow.github.io*
- **twitter_username**: Your twitter username. This will appear as a link to your twitter profile in the footer. 
- **github_username**:  You github username. This will appear as a link to your twitter profile in the footer. In this demo it will be *TeamArrow*
- **theme**: This is the Jekyll theme that will be used to style the site. Github supports some straight out of the box. You can check them out here: [https://pages.github.com/themes/](https://pages.github.com/themes/). There are other themes and I'll cover those in a later post

So if you set those things up for yourself, you should have a blog page that looks kinda like it's your blog. 
We aren't done yet though.

Something important to note here: **If you change _config.yml, you will need to kill and restart the jekyll serve to see your changes in your browser**

## The About Page, and other pages

As you can see, in the top right you have an About link on your site. When you click it, you see the default *about* page for your blog. To edit that, we can open *about.markdown*

At the top of the *About.markdown* file you will see a little section of YAML markup. This describes the page to jekyll.

``` yml
---
layout: page
title: About
permalink: /about/
---

```

The **layout** specifies the layout to use to render the page. The list of valid options for this are dependent on your theme. 
The **title** specifies the big &lt;H1&gt; at the top of your page. 
The **permalink** gets controls where jekyll outputs the actual HTML file so you can create links to it directly. 
There are other yml properties we can put at the top to control the rendering and the theming. 

All pages in Jekyll will have the YML section at the top. If you make more pages, you just need to copy and paste it from another file. 

Everything below the YML on the page is markdown that will be used to render the page. 

To create a new page on the site, you can just make a new markdown file and put the content in that you like. 

## Adding Blog Posts

There is a folder in your blog directory called *_posts*. This folder currently should contain a single markdown file.
Take a look at the name. 
It's effectively a date and a title in YYYY-MM-DD-the-post-title.markdown.
Jekyll uses the file name as a unique id for a post.
Once it has generated the html for a specific post filename it wont update stuff like the permalink.

So you can create a new file in that folder. Call it something like *2019-11-25-my-new-blog.markdown*.
Like pages, our posts need the YML section to describe them to Jekyll.
The existing post should have something like this in it:

``` yml

---
layout: post
title:  "Welcome to Jekyll!"
date:   2019-11-14 20:29:10 -0500
categories: jekyll update
---

```

So again:
- **layout** picks the page layout for our theme. 
- **title** is the big &lt;H1&gt; at the top of your post. 
- **date** is the publish date of the post. You set this, it's not worked out by Jekyll. You can put whatever you like here and the post ordering will be based on this. 
- **categories** is a little different. This is used to create the URL for your blog post. I set all of mine to *posts*. Checkout the Url for this post, you'll see posts in there.
  
You can add to these:
- **permalink**: you can set a permalink for a post, just like a page.
- **excerpt**: This is a short description of the post. In some themes this shows as a summary of a post on the home screen. It's also used in social links.
- **tags**: This is an array of values to tag the post. It's helps with search pages and tag clouds. This post has ``` tags: [github, blogging, themes, social, jekyll] ```
- **toc**: If your theme supports it, this will generate a Table of Contents somewhere on the page for the post. This will be based on the headers you use in the post. This can be controlled globally in the *_confi.yml* file
- **comments**: If you have comments enabled, it will render a comments box on the page. This can also be controlled globally in the *_confi.yml* file

There are other things you can put in here, a lot depend on the theme you use and what it allows to be customized. That's all I'm currently using.

So to add more posts to your blog. You just need to keep adding new markdown files in this directory, named like the others with the date and the title. When you commit them and push them to github, they'll show up.

## Images

I have images in my blog, it makes it easier to show things sometimes.
I created an *Images* folder in my blog folder and I store them in there. 
There isn't really a rule from Jekyll about how these should be named or structured. 
I choose to make a folder that's the same as a posts name for post images. 
Then I put them all in there for each post. 

For images generally for the blog, like my picture, I just put that in *images*.

From each post you can put an image in the markdown.

``` markdown
![alt text for image](/images/imagename.png)
```

The challenge here is that, */images/imagename* works on the final blog, but might not work in the previewer for markdown, especially for thinks with permalinks etc.
You'll need to pay attention to structuring the links properly. I tend to insert them with **../images/imagename** so it renders in the preview, then when the post is finished do a find and replace to change them absolute paths.


## Wrap Up

Thats all there is to it for basic editing. Using the [previous post](https://ciaranodonnell.dev/posts/setting-up-a-blog-with-github-pages/) to get started and this post you should be able to get a blog up and running. 

In the near future I'll write more about things I have learn't in my process of blogging.
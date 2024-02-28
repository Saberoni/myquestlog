---
title: Fixing Hugo baseURL
date: 2024-02-27
description: |
  How to fix absolute urls in Hugo when deploying to Cloudflare Pages,
  and without breaking local development
hero: images/posts/cfpages/cfpages-banner.png
menu:
  sidebar:
    name: Fixing Hugo baseURL
    identifier: hugo-baseurl
    parent: CloudflarePages
    weight: 11
tags:
- Hugo
- Cloudflare
---

## Introduction
When deploying to Cloudflare Pages I noticed a lot of links were broken and a quick searched revealed that 
there are problems with the baseURL and Cloudflare Pages.

I'll go through the steps I did to make sure that links to Posts, Notes, and some of the social links don't get broken. 

## The Problem

So you've kept things standard and in your `hugo.yaml` you have set your base url to `baseURL: /`.

However, when developing locally you will notice that some links are broken e.g.

{{< img src="/posts/cloudflare/cfpages/localhost-url.png" title="Broken URL" >}}

Additionally, since this post is specifically about Cloudflare Pages, assuming you have followed their documentation then your
build command would look something like this `hugo -b $CF_PAGES_URL`.

That's all well and good but what you will find is that variable actually ends up using your commit id in the url, so although your site might look ok you will find some links don't work. I found this out because I enabled Access Policies on my preview builds, then when I followed a link on my site I was a bit confused about why I was being asked for a code. 

## A Solution

Now while there are multiple ways to get around this such as scripts and changing absolute urls in the code, I opted to keep it simple. 

Fist the hugo config.

I set the baseURL in `hugo.yaml` to the url of my production site. e.g. `baseURL: https://myquestlog.pages.dev/`.

Don't worry, this won't break local development since when you use the command `hugo server` locally it sets the baseURL to localhost.

Now on the Cloudflare end, make sure your build command is as recommended. `hugo -b $CF_PAGES_URL`

Then finally, on your Pages Settings tab, go to Environment Variables and set the following Production var, it doesn't need to be encoded.

**Variable name = CF_PAGES_URL**, 
**Value = https://myquestlog.pages.dev/**

Make sure to set the value to your production sites url.

Doing this will make it so that preview builds work as expected, and the production build won't end up using the commit url in any relative urls. 

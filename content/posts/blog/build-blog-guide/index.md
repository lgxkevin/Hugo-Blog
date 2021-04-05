---
weight: 2
title: "Building a blog with Hugo and Netlify"
date: 2021-02-27T15:52:24+13:00
draft: false
description: "Build a personal blog with Hugo within an hour."
resources:
- name: "featured-image"
  src: "featured-image.jpg"

tags: ["Hugo", "Netlify"]
categories: ["Blog"]

lightgallery: true
---

Build a personal blog with Hugo within an hour.

<!--more-->

## Start with Hugo
### Step 1: Install Hugo

On **Windows**

This video is the tutorial I followed to install Hugo on Windows 10.

{{< youtube G7umPCU-8xc >}}

{{< admonition note "Summary of steps in the above video" false>}}
  - Create a new folder under C drive 
  - Download the zip file from the Hugo repo on Github
  - Unzip file under the folder just created
  - Set up environment path
{{< /admonition >}}

On **other** operating systems

Please follow the guide [here](https://hugoloveit.com/basic-markdown-syntax/#links "Install hugo")

### Step 2: Pick up a theme

This website is using [this theme](https://hugoloveit.com/theme-documentation-basics/ "Theme reference")

{{< admonition tip "Main reasons why I choose this theme:" false >}}
:smirk: Dark theme, programmers' favourite :wink:
{{< /admonition >}}

### Step 3: Hosting on Netlify

Please click this [link](https://hugoloveit.com/basic-markdown-syntax/#links "Setup with Netlify") to setup Netlify

{{< admonition note "Why to use Netlify?" true>}}
- UI friendly and easy to follow
- Cost-efficient, the free plan is enough
- Only a few steps to set up Hugo blog
- Easy to set up a custom domain
{{< /admonition >}}

## Summary

Some **holes** :cyclone: I met when I was building this blog:

- Not all markdown syntax supported in Hugo, such as the collapse list
- Different Hugo themes will have different setup steps, features, and syntax, so there isn't a single way to make all themes work perfectly. Please try two or three themes and pick up the one you are comfortable with.
- Make sure you use the latest Hugo version, some themes and features might not compatible with the old Hugo version.

**Tools** :hammer: check list:
- Hugo ---> Static website framework
- Netlify ---> Hosting and deploying the website
- LoveIt ---> Hugo theme
- Visual Stuio code ---> code editor
- Github ---> Code repository

At the **end** :bulb:

The way to build a personal blog website is easy, but keep writing a blog is hard.
Hope you and especially **me** always remember the reason why we build our blog.

## Further references

[Markdown reference](https://hugoloveit.com/basic-markdown-syntax/#links "Markdown reference")

[Theme reference](https://hugoloveit.com/theme-documentation-basics/ "Theme reference")

[Host on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/ "Host on Netlify")

[Theme Demo source code](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/ "Theme Demo code")

[Emoji cheat sheet 1](https://gist.github.com/rxaviers/7360908 "Emoji cheat sheet")
[Emoji cheat sheet 2](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md "Emoji cheat sheet")

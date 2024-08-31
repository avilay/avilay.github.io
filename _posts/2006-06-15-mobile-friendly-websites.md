---
layout: post
title: Mobile Friendly Websites
date: '2006-06-15 07:00:00'
---

Browsing on my pocketPC and smart phone has one big problem. Most of the websites I browse are not built to render on a small screen with limited bandwidth. It is painful to download and painful to render. A possible solution would be to build a middle-man gateway website, which will accept a website request from me, get the web page from the actual website, strip off useless content and only render the main content to my mobile.

The fun part is build the engine that strips out non-relevant portion. How does it recognize non-relevant portions. I could probably use very sophisticated statistical ML algos to do this (I havent figured out which one is best), or as a brute force technique - build an artificial neural network. A very simple approch would be to select the "middle" element in the web page and display that. Actually determining what is middle in an HTML would be quite challenging to begin with. I should check out if such websites already exist.

Another variation would be to get all my favourites and build a mashup of those and apply these techinques specifically to those websites. That would be easy enough. At lest my usage pattern is that I go to a very small list of websites on my mobile. I dont use it for general surfing. Maybe I should measure my browsing pattern and based on that build something.

Of course I dont have to build a website, I could just build a pIE plugin to do this for depending on how resource intensive this is algo is...it might just work to have it embeded in pIE.


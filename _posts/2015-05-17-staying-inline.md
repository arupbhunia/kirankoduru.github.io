---
layout: post
title:  "Convert style tags to inline css for emails"
date:   2015-05-17 22:40:12
categories: javascript
description : To parse style tags in your HTML add them as inline css.
image: /img/staying inline.png
---

While working on some email templates last week I had a nightmare styling HTML for different email clients. GMail definitely doesn't allow `<style>` tags in their email markups. The safest best was to include __inline style__ attributes which renders the templates correctly for all email clients.

So here I present you a tool to move the style tags from the HTML to their inline equivalents. 

Paste the HTML of your email template in the left box and get the output with inline style attributes in the right. 
<a href="http://kirankoduru.github.io/stayin-inline/">
![Stayin inline]({{ site.baseurl }}/img/staying inline.png)
</a>

You can also checkout the source code on github [here](https://github.com/kirankoduru/stayin-inline). 

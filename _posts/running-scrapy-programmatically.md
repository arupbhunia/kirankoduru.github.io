---
layout: post
title:  "Running multiple scrapy spiders"
date:   2015-1-16 20:17:00
categories: python
description : My idea for running multiple scrapy spiders programmatically.
---
This is my first post this year, so Happy New Year !

I wanted to share something that I have been working on for the past few months which is running python scrapers using the [scrapy](http://scrapy.org) framework. There are so many fascinating things in it that led me to believe that this is it! My team was hooked to it and start reading the [docs](http://scrapy.readthedocs.org) daily on how to get it perfect. We also started hiring people based solely on their effeciency of using scrapy.

Today I am going to share how I started running multiple spiders in scrapy. A regular scrapy project consists of spiders and its individual scraper settings. We started to learn that we have to 

{% gist kirankoduru/fef98acfcf4069057152 %}
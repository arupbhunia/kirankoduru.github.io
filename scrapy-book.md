---
layout: about
title: Web Scraping with Python
permalink: /webscraping-with-python/
description: Learn the art of extracting content and building web crawlers with python. This book will walk you through the process of building an infrastructure that allows you to write and manage a large ecosystem of web scrapers written in python and the Scrapy framework.
---

Through my short career as a software developer I happen to learn by doing a lot of things. I believe, this knowledge that was acquired from a beginners perspective is useful to everyone who wants to progress their careers slowly as software engineer.

### About the Book

Many tutorials and blogs have been published on how to parse HTML and gather information from the web with python but only a few have been able to walk you through the process from scratch to building a massive web crawling infrastructure. The intention of this book is to guide beginners on extracting text from the web using the best tools available in python.
 
Whether you are building your own search engine or just scraping the web for products from multiple shopping websites, it is important to learn the caveats of your infrastructure. This book will teach you about how to create analytics, monitor your environment and scale your scraping infrastructure steadily.


### Who is this book for

This book for anyone who has a basic understanding of python and wants to get started with web scraping.
 
This book is also for someone who is not familiar with the python Scrapy framework and would like to expand their knowledge on the topic.


### Chapters

An overview of what to expect from the book, this is not the complete list but I am beginning to think of topics in these direction. Would love to know your thoughts and comments.

#### 1. Python and Web Scraping

An introduction to python and some concepts that will be useful as you progress through the book. I know it sounds like just another book that doesn't jump into the guts of it but I promise I'll only cover some basics to get started.

#### 2. Learning xpath and css extraction

Screen scraping is all about `css` and `xpath` selectors. You need some refreshers on how to pull up that browser debug tool or the web console. This chapter will walk you through fetching data from a web page. 

#### 3. Beautifulsoup and Python Requests/urllib

Webscraping with python means you need to learn to work with the `urllib`, `requests`, `lxml` and `Beautifulsoup` libraries. Setting up your session and cookies to maintain state across requests will teach you to navigate a website without logging in each time.

#### 4. Parsing the DOM and working with databases

A short chapter on how to iterate through your DOM objects and interact with databases. The more data you gather, the more you will want to structure your data initially. In the chapter you'll learn to design your schema to hold and retrieve data.

#### 5. Introduction to Scrapy

The open source framework for every python developer. Though `async` and `await` can help you design the moving parts for your own web crawling infrastructure, __Scrapy__ was built on the shoulders of giants who work with web scraping day to day.

#### 6. Scrapy Pipelines and Extensions

With the wonderful world of __Scrapy__ comes a ecosystem of tools that are pluggable. Learn to build your own pipelines and extension for cleansing your data, validating and logging.

#### 7. Scrapy Core API

In this chapter we will walk through the core architecture of the __Scrapy__ framework. If you were to know the guts of a system you work with you will be skilled to make modifications as need be.

#### 8. Scrapy Splash and other headless browsers

Not all data can be scraped by screen scraping. For DOM that is generated using javascript, you need a headless browser to parse it. Most headless browsers that you might have come across in the python use the `selenium` web driver to interact with them. Little do you know that the __splash__ headless browser built specifically for __Scrapy__ is highly configurable and programmable with your existing __Scrapy__ spiders.

#### 9. scrapyd and scrapinghub.com and DIY REST API

Once you have written your scrapy spiders you will need to learn to configure them to run. You could very well use `crontabs` to run them periodically but as the infrastructure grows, you will need to maintain them more efficiently. `scrapyd` or __Scrapy__ daemon is one of the ways you can deploy your spiders.

#### 10. Scrapy logs and analytics

Each of your scrapy spiders run have statistics generated after each run. You can use these stats to build a analytics dashboard. We will focus on analyzing this data with `elasticsearch` and `kibana`.

#### 11. Crawling Responsibly

When writing a web crawler you are hitting someone else's resource that can affect them. You have to comply with their `robots.txt` files and mark your __User Agent__ strings appropriately.

#### 12. Scrapy Near Real-time and cluster

From building your API for running Scrapy spiders to deploying a cluster that shares a queue of requests among each other. You will learn to build a cluster that can be deployed with `docker` containers.

<div class="signup-form">
    <div class="float-left" style="width:23%;padding:4%">
        <img src="../img/webscraping-with-python-book.png">
    </div>
     <div class="float-right" style="width:67%;padding-top:1%;padding-right:2%;text-align: left;">
        <h2>Like what you read about the book so far?</h2>
        <p>Are enthused with the topics on the book? Are looking to build web scrapers at scale? Or do you wish to just receive more anecdotes on python from this blog then please signup to the email list below.</p>
    </div>
    <div class="clear-both"></div>
    <div class="input-form">
      <form action="https://feedburner.google.com/fb/a/mailverify"
          method="post" target="popupwindow" onsubmit="window.open('https://feedburner.google.com/fb/a/mailverify?uri=kirankoduru', 'popupwindow', 'scrollbars=yes,width=550,height=520');return true">
      <input type="text" name="email" placeholder="Please enter your email address"/>
      <input type="hidden" value="kirankoduru" name="uri"/>
      <input type="hidden" name="loc" value="en_US"/>
      <input class="button cursor pointer" type="submit" value="Sign Up Now" />
      </form>
    </div>
</div>

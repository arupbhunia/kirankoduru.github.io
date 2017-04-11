---
layout: post
title:  "Running scrapy spider programmatically"
date:   2015-01-24 13:00:18
updated: 2017-04-09 15:03:12
categories: python
description : My idea for running a scrapy spider programmatically.
note: This post refers to using scrapy version 0.24.4, if you are using a different version of scrapy then refer <a href="https://doc.scrapy.org/en/latest/topics/practices.html#running-multiple-spiders-in-the-same-process">scrapy docs</a> for more info. Also this blog post series received a lot of attention so I created a pip package to make it easy to run your scrapy spiders. Please check the project on <a href="http://github.com/kirankoduru/arachne">github</a>.
---
I wanted to share something that I have been working on for the past few months, which is, running scrapers with the [scrapy](http://scrapy.org) framework. I understand that scrapy has existed for many years, but it is still so relevant and useful for me and my team. We were hooked to it and started reading the [docs](http://scrapy.readthedocs.org) daily on how to get it perfect. There are two ways of running a scrapy spider. You can run a scrapy spider from the command line or using a program.

Today, I am going to illustrate how to use the framework by running it by using its [Core API](http://scrapy.readthedocs.org/en/latest/topics/api.html). If you are not familiar with how web scraping works and would like to use scrapy to get you started, then you should definitely look into [this tutorial](http://scrapy.readthedocs.org/en/latest/intro/tutorial.html).

What you would need to know before we start are:

+ __The Scrapy Spider__ : It is a python class in the scrapy framework that is responsible for fetching URLs and parsing the information in the page response.

+ __Your Custom Spider__ : It extends the scrapy spider class. We implement the method `parse` to be able to parse the page response. In the example below `DmozSpider` is the custom spider.

+ __The Scrapy item__ : It is an object that will act as a dictionary to store all the information you want to parse.

+ __The Scrapy Selector__  : To select elements on the page with an xpath selector or a css selector. In older versions of scrapy you had to import the `Selector` class but now you can use the selectors on the `response` object directly.

I am going to use the example from [scrapy tutorial](http://scrapy.readthedocs.org/en/latest/intro/tutorial.html) to make it easy to understand.

This is what the spider file __DmozSpider.py__ looks like:
{% highlight python %}
import scrapy

class DmozItem(scrapy.Item):
    title = scrapy.Field()
    link = scrapy.Field()
    desc = scrapy.Field()

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/",
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Resources/"
    ]

    def parse(self, response):
        for sel in response.xpath('//ul/li'):
            item = DmozItem()
            item['title'] = sel.xpath('a/text()').extract()
            item['link'] = sel.xpath('a/@href').extract()
            item['desc'] = sel.xpath('text()').extract()
            yield item
{% endhighlight %}

To be able to run this spider solely from scrapy core script:
{% highlight python %}
# import dmoz spider class
from DmozSpider import DmozSpider

# scrapy api
from scrapy import signals, log
from twisted.internet import reactor
from scrapy.crawler import Crawler
from scrapy.settings import Settings

def spider_closing(spider):
    """Activates on spider closed signal"""
    log.msg("Closing reactor", level=log.INFO)
    reactor.stop()

log.start(loglevel=log.DEBUG)
settings = Settings()

# crawl responsibly
settings.set("USER_AGENT", "Kiran Koduru (+http://kirankoduru.github.io)")
crawler = Crawler(settings)

# stop reactor when spider closes
crawler.signals.connect(spider_closing, signal=signals.spider_closed)

crawler.configure()
crawler.crawl(DmozSpider())
crawler.start()
reactor.run()
{% endhighlight %}

You can also add a pipeline to insert the item into your database by using the `ITEMS_PIPELINE` in the scrapy settings. I will illustrate that in my next blog post and also how you will be able to run 2 spiders parallely [here](http://kirankoduru.github.io/python/multiple-scrapy-spiders.html).

[Download Project](https://github.com/kirankoduru/scrapy-programmatically/archive/7ae412cfd00968150fcd0aa847949c15df6c8c39.zip) or [View on github](https://github.com/kirankoduru/scrapy-programmatically/tree/7ae412cfd00968150fcd0aa847949c15df6c8c39)

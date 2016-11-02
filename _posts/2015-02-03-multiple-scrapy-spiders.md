---
layout: post
title:  "Running multiple scrapy spiders programmatically"
date:   2015-02-03 22:13:42
categories: python
description : Running multiple spiders from a script programmatically
note: This post refers to using scrapy version 0.24.4, if you are using a different version of scrapy then refer <a href="http://scrapy.readthedocs.org">scrapy docs</a> for more info. Also this blog post series received a lot of attention so I created a pip package to make it easy to run your scrapy spiders. Please check the project on <a href="http://github.com/kirankoduru/arachne">github</a>.
---
This is a continuation of my [last post](http://kirankoduru.github.io/python/running-scrapy-programmatically.html) about how to run scrapers from a python script. In this post I will be writing about how to manage 2 spiders. You can run over 30 spiders concurrently using this script.

Right now, this is how I have organise my project.
{% highlight bash %}
├── core.py
└── spiders # the spiders directory with all the spiders
    ├── CraigslistSpider.py
    ├── DmozSpider.py
    └── __init__.py

{% endhighlight %} 

As you can see, I have an additional spider that will be part of this program. Again, remember, we will be able to add over 30 spiders through this script. The logic remains the same. We will focus on how to get 2 spiders running right now.

{% highlight python %}
# import the spiders you want to run
from spiders.DmozSpider import DmozSpider
from spiders.CraigslistSpider import CraigslistSpider

# scrapy api imports
from scrapy import signals, log
from twisted.internet import reactor
from scrapy.crawler import Crawler
from scrapy.settings import Settings


# list of crawlers
TO_CRAWL = [DmozSpider, CraigslistSpider]

# crawlers that are running 
RUNNING_CRAWLERS = []

def spider_closing(spider):
    """
    Activates on spider closed signal
    """
    log.msg("Spider closed: %s" % spider, level=log.INFO)
    RUNNING_CRAWLERS.remove(spider)
    if not RUNNING_CRAWLERS:
        reactor.stop()

# start logger
log.start(loglevel=log.DEBUG)

# set up the crawler and start to crawl one spider at a time
for spider in TO_CRAWL:
    settings = Settings()

    # crawl responsibly
    settings.set("USER_AGENT", "Kiran Koduru (+http://kirankoduru.github.io)")
    crawler = Crawler(settings)
    crawler_obj = spider()
    RUNNING_CRAWLERS.append(crawler_obj)

    # stop reactor when spider closes
    crawler.signals.connect(spider_closing, signal=signals.spider_closed)
    crawler.configure()
    crawler.crawl(crawler_obj)
    crawler.start()

# blocks process; so always keep as the last statement
reactor.run()

{% endhighlight %}

My little hack here is to make a list of `RUNNING_CRAWLERS` and on the spider `signal_closed`, which is sent by the spider when it stops I remove them from the `RUNNING_CRAWLERS` list. And finally when there are no more spiders in the `RUNNING_CRAWLERS`, we stop the script.

You can view my last post in this series about how to implement the scrapy pipeline to insert items into your database [here](http://kirankoduru.github.io/python/sqlalchemy-pipeline-scrapy.html).

[Download Project](https://github.com/kirankoduru/scrapy-programmatically/archive/487311b2e48a5c4e712237f61e9a97ff1540ddb2.zip) or [View on github](https://github.com/kirankoduru/scrapy-programmatically/tree/487311b2e48a5c4e712237f61e9a97ff1540ddb2)

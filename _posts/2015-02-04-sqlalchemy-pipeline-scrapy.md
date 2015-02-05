---
layout: post
title:  "SQL Alchemy pipeline to add item to DB"
date:   2015-02-04 22:42:46
categories: python
description : Scrapy pipelines for inserting item into the database.
---

In my [last post](http://kirankoduru.github.io/python/multiple-scrapy-spiders.html), I talked about how to run 2 spiders concurrently. This post is a brief introduction to how to add scrapy items into database through the pipeline.

The scrapy framework is magnificient when it comes to data processing. There are tons of features that it uses and lets developers configure. Since we are using the core API to run our scrapers right now, we are able to set the pipeline using

{% highlight python %}
settings.set("ITEM_PIPELINES", {'pipelines.AddTablePipeline': 100})
{% endhighlight %}

The `ITEM_PIPELINES` is a python dictionary with the _key_ as the location of the pipeline object and the _value_ as the order in which the key should run. We can assign numbers between __100-1000__ to run the pipelines. In this project I am basically creating a database called __scrapyspiders__ for which I set the settings [here](https://github.com/kirankoduru/scrapy-programmatically/blob/master/database/connection.py#L6-L9). I create the connections and create the SQL Alchemy ORM model with the fields _id_, _title_, _url_ and _date_.

{% highlight python %}
from sqlalchemy import Column, String, Integer, DateTime
from database.connection import Base

class AllData(Base):
    __tablename__ = 'alldata'

    id = Column(Integer, primary_key=True)
    title = Column(String(1000))
    url = Column(String(1000))
    date = Column(DateTime)

    def __init__(self, id=None, title=None, url=None, date=None):
        self.id = id
        self.title = title
        self.url = url
        self.date = date

    def __repr__(self):
        return "<AllData: id='%d', title='%s', url='%s', date='%s'>" % (self.id, self.title, self.url, self.date)
{% endhighlight %}

I call the SQL Alchemy object in the items pipeline and create a _record_ and insert the new item into the database with the corresponding values from the item.

{% highlight python %}
from database.connection import db
from database.models import AllData

class AddTablePipeline(object):

    def process_item(self, item, spider):
        if item['title'] and item['url']:
            if 'date' not in item or not item['date']:
                date = None
            else:
                date = item['date'][0]

            # create a new SQL Alchemy object and add to the db session
            record = AllData(title=item['title'][0].decode('unicode_escape'),
                             url=item['url'][0],
                             date=date)
            db.add(record)
            db.commit()
            return item

{% endhighlight %}

And finally run using the command:

{% highlight bash %}
python core.py
{% endhighlight %}

[Download Project](https://github.com/kirankoduru/scrapy-programmatically/archive/0b20f674da3e263c134dff34171aa63d26fd5868.zip) or [View on github](https://github.com/kirankoduru/scrapy-programmatically/archive/0b20f674da3e263c134dff34171aa63d26fd5868)
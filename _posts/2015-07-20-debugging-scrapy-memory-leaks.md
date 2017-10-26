---
layout: post
title:  "Debugging scrapy memory leaks"
date:   2015-07-20 19:22:22
updated: 2017-10-25 18:40:57
categories: python
description : How do you debug scrapy's memory leak?
---
The only reason I perceive that you will read this is cause you have issues with a Scrapy spider leaking memory. For others I would say, you can bookmark it for when your Scrapy spider starts leaking memory.

### Did you know you could telnet into scrapy spiders? 
If you have been watching closing at your scrapy console you will see a line that tells you the port that the scrapy spider has a telnet console running on.

```
2015-07-20 20:32:11-0400 [scrapy] DEBUG: Telnet console listening on 127.0.0.1:6023
```

Well, when you see the port that your spider is listening on then you can __telnet__ into your spider using the command line. The port I will telnet into is __6023__. 

<pre>
telnet localhost 6023

# this connects to the port 6023 and use the command prefs()
# to list all details about your spider

>>> prefs()

Live References

# scrapy class                 Memory   Time ago
HtmlResponse                        3   oldest:   5s ago
CraigslistItem                    100   oldest:   5s ago
DmozItem                            1   oldest:   0s ago
DmozSpider                          1   oldest:   6s ago
CraigslistSpider                    1   oldest:   5s ago
Request                          3000   oldest: 705s ago
Selector                           14   oldest:   5s ago
</pre>


Clearly, the issue is with one of my `Request` objects. Quickly going back to my code I saw that I used `yield Request` a lot which caused many objects to be created in memory. 

Tell me how you solved your memory leaks with Scrapy in the comments below or post a question below if you would like help debugging your spiders.

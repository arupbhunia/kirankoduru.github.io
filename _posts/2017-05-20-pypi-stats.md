---
layout: post
title:  "How to get PyPI download statistics"
date:   2017-05-20 14:55:43
categories: python
description : How to get how recent package statistics from PyPI through bigquery
author: Kiran Koduru
image: /img/bigquery.jpg 
---
This is a short post on how to get download statistics about any package from PyPI. Though there have been efforts in that direction from sites like [pypi ranking](http://pypi-ranking.info) but this post finds a better solution.

Google has been generous enu canough to donate it's Big Query capacity to the Python Software Foundation. You can access the pypi downloads table through the [Big Query console](https://bigquery.cloud.google.com/table/the-psf:pypi.downloads). I ran a sample query to find out how my personal package [arachne](https://pypi.python.org/pypi/Arachne) has been doing on PyPI.

{% highlight sql %}

SELECT COUNT(*) as download_count
FROM TABLE_DATE_RANGE(
  [the-psf:pypi.downloads],
  TIMESTAMP("2015-05-01"),
  CURRENT_TIMESTAMP()
)
WHERE file.project="arachne"

{% endhighlight %}
_The above query returns the download count since 1st May 2015_

Thanks to [Ofek Lev](https://github.com/ofek) on his package [pypinfo](https://github.com/ofek/pypinfo) for inspiring me to write this post.

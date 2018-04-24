---
layout: post
title:  "Periodically running python programs"
date: 2018-04-23 20:28:58
categories: python
description : Scheduling python programs to run at regular intervals
author: Kiran Koduru
---

When I learned to program, one of the things I wanted to do was run tasks, jobs, functions, programs etc. on a periodic basis. On my journey with python I learned that there are multiple techniques to accomplish this. I learned that I could run a program in the background in the command line or run it as a [cron](https://en.wikipedia.org/wiki/Cron). There are also a combination of python packages that are available which can be useful in running background tasks. I will try to walk to some of them during this post. Forewarning, this is a long post so if you would like to put a pin in it and comeback later, I totally understand. I will try and break in down in multiple posts so it's easier to come back to. For beginners to python and Unix I would recommend reading the whole series in order to get an idea of working with periodic tasks in python or Unix environments.

Before we begin, have you ever wondered, after you have done writing a program, how would you go about trying to run the program periodically? In my earlier days when I started to program, I was excited I wrote a program that worked but, I wanted to run it at regular intervals. One such program I wrote was scraping data from multiple websites. I wondered, how could I scrape information periodically in-order to have the freshest data always. I'll try and explain in a series of posts how would you go about setting up running a basic cron to a bit of advanced workflows used in python that simulate periodic tasks via celery. So I hope this helps you in some ways.

I need to start with a program. Something that can be run as background task. My side project needed emails to be sent out periodically, so what better way than automating the schedule for the emails to be sent.

I created a module named _send_emails.py_. In python files are called _modules_ and _packages_ are folders with files. Weird, I know. So, my module emails the latest blog post to an email list and if there aren't any new posts published, then I won't be sending out any emails. 

I will use the python [requests](http://docs.python-requests.org/en/master/) package to send emails via the [mailgun API](https://www.mailgun.com/). And [SQLAlchemy](https://www.sqlalchemy.org/) as the ORM to for my MySQL database. 

My database has 2 tables, namely `users` and `blogpost` that I need to get data from. The `user` table has fields `id`and `email`, whereas the `blogpost` table has fields `id`, `title`, `body` and `published_date`.

Just incase you are wondering here's what the 2 tables look like

{% highlight bash %}
mysql> describe users;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| email | varchar(256) | NO   |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)

mysql> describe blogpost;
+----------------+----------------+------+-----+-------------------+----------------+
| Field          | Type           | Null | Key | Default           | Extra          |
+----------------+----------------+------+-----+-------------------+----------------+
| id             | int(11)        | NO   | PRI | NULL              | auto_increment |
| title          | varchar(500)   | NO   |     | NULL              |                |
| body           | varchar(10000) | NO   |     | NULL              |                |
| published_date | datetime       | NO   |     | CURRENT_TIMESTAMP |                |
+----------------+----------------+------+-----+-------------------+----------------+
4 rows in set (0.00 sec)
{% endhighlight %}

Next, my module _send_email.py_ that'll send the new blog post to the email list in `users` table.

{% highlight python %}
# send_emails.py

import requests
from db_connection import db
from models import User, Blogpost
from sqlalchemy import desc

API_URL = "https://api.mailgun.net/v3/samples.mailgun.org/messages"
API_KEY = 'XXXXXXX'

def find_new_blogpost():
    post = db.query(Blogpost).order_by(desc(Blogpost.published_date)).first()
    return post

def get_mailing_list_users():
    all_users = db.query(Users).all()

    # mailgun accepts a comma separated list of emails
    comma_separated_emails = ','.join([user.email for user in all_users])
    return comma_separated_emails

def send_emails():
    from_addr = "Yours Truly <whoami@example.com>"
    mailing_list = get_mailing_list_users()
    new_blogpost = find_new_blogpost()

    # only send emails if the post hasn't been sent before
    if not new_blogpost.sent:
        metadata = {"from": from_addr,
                    "to": mailing_list,
                    "subject": new_blogpost.title,
                    "text": new_blogpost.body}

        resp = requests.post(API_URL,
                             auth=("api", API_KEY),
                             data=metadata)

        # only update the post status to `sent` if mailgun sends a 200 response
        if resp.status_code == 200:
            new_blogpost.sent = True
            db.commit()

if __name__ == '__main__':
    send_emails()

{% endhighlight %}

Now this is what my directory structure looks like

<pre>
.
└── my_emailer
    ├── __init__.py
    ├── db_connection.py
    ├── models.py
    └── send_emails.py
</pre>

### Running program in the background

The novice implementation of running the above program as a cron would be to run the program in the backgroud. You could add a `while True` loop with a delay using python's `time.sleep()` function. I will add a 5 second delay for sending emails. You can probably calculate the time in seconds you would like to send the email at for larger intervals.

{% highlight python %}
# Filename: send_emails_bg.py

from time import sleep

...

if __name__ == '__main__':
    while True:
        sleep(5) # sleeps for 5 seconds
        send_emails()

{% endhighlight %}

To run the program above on Unix I added the `&` symbol when calling it and it'll run in the background.

<pre>
python send_emails_bg.py &
</pre>

To view the list of running programs, I used the `jobs` command. I can also bring it to the foreground with the `fg` command and finally quit the program with the `Ctrl + C` command.

<pre>
user@home:~/test_project $ python send_emails.py &
[1] 19173
user@home:~/test_project $ jobs
[1]+  Running              python send_emails.py &
user@home:~/test_project $ fg
^C
</pre>

Or if I am not interested in coming back to the process then I could use [`nohup`](https://en.wikipedia.org/wiki/Nohup).

<pre>
nohup python send_emails_bg.py &
</pre>

Running my program like this would probably be okay to test something out but I wouldn't recommend doing this for larger programs. My process would definitely stop running if my machine was rebooted. Hence the alternative, using __cron__!   

### Setting up the cron

Cron gives me the assurance that the program will run daily, hourly, weekly or monthly. It wouldn't be affected by reboots. I could either edit the existing cron tab file with `crontab -e` command or edit the `/etc/crontab` file on my Unix environment. 

<pre> 
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
  * 7 * * *   admin    cd /my/path/to/folder;python send_emails.py >> test.log 2>&1
</pre>

Now the above command will run as the user `admin` at the 7th hour of the day(i.e. 7 am) daily. The command will pipe the `stdout` and the `stderr` to the __test.log__ file. 

If you are looking for an implementation on Windows then you can probably find some answers [here](https://stackoverflow.com/questions/7195503/setting-up-a-cron-job-in-windows).

Also, `cron` has other implementations that I haven't looked into like [Anacron](https://en.wikipedia.org/wiki/Anacron). From what I have seen it allows to schedule tasks to run on next reboot if the system is turned off. Might be useful for someone who wants to run their job atleast once even if not on schedule.

Hope this tutorial was useful in a way to get to start with __cron__ and running code in the background. In my next post, I will talk about how to daemonize your python code so you can run it as a service.

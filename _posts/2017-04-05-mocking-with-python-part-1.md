---
layout: post
title:  "Mocking with Python - Part I"
date:   2017-04-05 23:21:23
updated: 2017-11-02 00:34:01
categories: python
description : Learning to mock in python
author: Kiran Koduru
image: /img/hare.png
---

If you have been following my blog, you may have noticed a pattern; I write a lot about testing software. Recently, I picked up a book on [Building Micro Services](http://shop.oreilly.com/product/0636920033158.do), where I came across a chapter on testing micro services. This post is about mocking your python code to make it easier to test your micro services or the likes.

There are a lot of posts to describe the need for mocking. I won't be getting into that discussion, though I do want to help you get started with mocking functions and objects in python. I found that when I tried to learn about mocking, there weren't a lot of introductory tutorials for new comers and this blog post is a reflection of what I think might be useful to learn mocking.

Let's start with a basic example where you fetch some rows of data from a database. If you were to write some python code for it, you will start by making a database connection through a ORM like [SQLAlchemy](https://www.sqlalchemy.org/). Once the connection is established, you will write SQL queries using the database object models. For the sake of simplicity, I will keep the code contained to:

- __get_all_users()__: A function  with returns a iterable object of `users` in the database
- __has_user_expired()__: A function which returns a `boolean` if the user has expired
- __find_expired_users()__: A function which returns a `list` of expired user IDs in our database

For our test case we will use the [python unittest](https://docs.python.org/2/library/unittest.html) framework. Each `user` returned by `get_all_users()` item is a SQLAlchemy object with the attributes `id`, `firstname`, `lastname`, `join_date` and `expiration_date`.

Here's how our application is structured. It contains just 3 files __db_connection.py__, __app.py__ and __test_app.py__.

<pre>
.
├── app.py              # applicaton code
├── db_connection.py    # database connection
└── test_app.py         # unit tests

0 directories, 3 files
</pre>

This is what our application code looks like

{% highlight python %}

# app.py
from datetime import datetime

def get_all_users():
    from db_connection import db
    results = db.query(Users).all()
    return results

def has_user_expired(user):
    if user.expiration_date < datetime.now():
        return True
    return False

def find_expired_users():
    expired_users = []
    users = get_all_users()
    for user in users:
        if has_user_expired(user):
            expired_users.append(user.id)
    return expired_users

{% endhighlight %}

We will write our test in the __test_app.py__ file. We will use mocks to avoid querying the database to fetch records via `get_all_users()`. For this we create a `Mock` object and assign it the attributes that a user has. This `Mock` object is just another python class when called for any fake attribute will return an answer.


> It's like a magic hat. You ask it for a variable, function, method etc. it returns a definite answer. You can also override the variable, function, method yourself so you know what is the expected result. 

In our case we will override the SQLAlchemy database object `user` and it's attributes `id`, `firstname`, `lastname`, `join_date` and `expiration_date`. So we can test for the `user` object even though we know it's not real.

{% highlight python %}

# test_app.py
import unittest
import datetime as dt
from mock import Mock, patch
from app import has_user_expired, find_expired_users

class TestApp(unittest.TestCase):

    def setUp(self):
        self.today = dt.datetime.now()
        self.yesterday = dt.datetime.now() - dt.timedelta(days=1)
        self.day_before = dt.datetime.now() - dt.timedelta(days=2)
        self.tomorrow = dt.datetime.now() + dt.timedelta(days=2)

    def test_has_user_expired(self):
        '''
        The function `has_user_expired()` takes a argument `user` which is a 
        SQLAlchemy database object. The object contains attributes `id`, 
        `firstname`, `lastname`, `join_date` and `expiration_date`.

        If the `expiration_date` is less than the date today it will 
        return `True` or if the `expiration_date` is greater than today 
        then return `False`

        For this test since we do not want to speak with the database we
        will create a `Mock` object and assign it the attributes that 
        a user has. This `Mock` object is just another python class that when
        called for any fake attribute will return an answer.
        '''

        # create a mock `user` object
        user = Mock()

        # now set the attributes for the `user` object
        user.id = 1
        user.firstname = 'John'
        user.lastname = 'Smith'
        user.join_date = self.day_before
        user.expiration_date = self.yesterday

        # now we pass the `user` object check for expected result `True`
        result = has_user_expired(user)
        self.assertTrue(result)

        # for the same user we can update the `expiration_date` to sometime
        # in the future to see if the expected result is `False`
        user.expiration_date = self.tomorrow
        result = has_user_expired(user)
        self.assertFalse(result)

    def test_find_expired_users(self):
        """
        We patch the function `get_all_user()` with our own function
        `mocked_get_all_users` for which we set a `return_value`.
        The `return_value` will contain a list of `Mock` users. 
        Some of these users are expired and some of them haven't expired. 
        We compare the output of our mocked `get_all_user()` function with 
        the `expected` output.
        """

        with patch('app.get_all_users') as mocked_get_all_users:
            mocked_get_all_users.return_value = [
                Mock(id=1, firstname='John', lastname='Smith1', 
                     join_date=self.today, 
                     expiration_date=self.tomorrow),
                Mock(id=2, firstname='John', lastname='Smith2', 
                     join_date=self.yesterday, 
                     expiration_date=self.tomorrow),
                Mock(id=3, firstname='John', lastname='Smith3', 
                     join_date=self.day_before, 
                     expiration_date=self.tomorrow),
                Mock(id=4, firstname='John', lastname='Smith3', 
                     join_date=self.day_before, 
                     expiration_date=self.yesterday),
                Mock(id=5, firstname='John', lastname='Smith3', 
                     join_date=self.day_before, 
                     expiration_date=self.today),
            ]
            results = find_expired_users()
            expected = [4, 5] # we know only user `id` 4 and 5 have expired
            self.assertEquals(results, expected)

if __name__ == '__main__':
    unittest.main()

{% endhighlight %}

Hope that gives you an idea of how to get started with mocking in python. A good practice is to run regular integration tests on the mocked data regularly. It's possible that though your tests pass the moment you have production data your website begins to crumble. Hence the integration tests keep a tab on your unknowns.

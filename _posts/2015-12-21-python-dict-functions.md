---
layout: post
title:  "Python functions as dictionary values"
date:   2015-12-21 21:23:23
categories: python
description : Using python functions with dictionaries
---
Pro pythonistas will tell you that you should actually be using python with [higher order functions](http://learnyousomeerlang.com/higher-order-functions) & as [first class functions](https://en.wikipedia.org/wiki/First-class_function). Passing around functions are any programming languages greatest attribute. Here's a trick to making function calls using python dictionaries.

Consider you have some functions `add`, `subtract`, `divide` and `multiply`. You could call these functions from a python dictionary like so:

{% highlight python %}
def add(x, y):
    return x+y

def subtract(x, y):
    return x-y

def divide(x, y):
    return x/y

def multiply(x, y):
    return x*y

x, y = 20, 10
operations = {
    'add': add,
    'subtract': subtract,
    'divide': divide,
    'multiply': multiply,
}

# add variables x & y
print operations['add'](x, y)
# 30

# subtract variables x & y
print operations['subtract'](x, y)
# 10

# divide variables x & y
print operations['divide'](x, y)
# 2

# multiply variables x & y
print operations['multiply'](x, y)
# 100
{% endhighlight %}

To use variable size of arguments you could call the `operations` dictionary with `*args`
{% highlight python %}
def add(*args):
    total = 0
    for i in args:
        total += i
    return total

def subtract(x, y):
    return x-y

x, y, z = 20, 10, 5
p, q, r = 30, 40, 50

operations = {
    'add': add,
    'subtract': subtract
}

# add variables x, y, z, p, q, r
print operations['add'](x, y, z, p, q, r)
# 155


# subtract variables x & y
print operations['subtract'](x, y)
# 10
{% endhighlight %}

I would love to hear what you think about this post, also know about your tricks to using python in the comments below.

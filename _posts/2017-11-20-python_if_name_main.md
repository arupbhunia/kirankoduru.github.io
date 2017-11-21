---
layout: post
title:  "Python's if __name__ == '__main__'"
date:   2017-11-20 16:12:14
categories: python
description : What is if __name__ == '__main__'
author: Kiran Koduru
---

When you are new to python, you may have noticed that the variables that are widely used in all projects are `__name__`  in conjunction with `__main__`.
 They might be the most mentioned dunder variables in Python projects on github. To be specific, it is mentioned __[6,436,149](https://github.com/search?q=if+__name__+%3D%3D+%22__main__%22+language%3APython&type=Code&utf8=%E2%9C%93)__ times at the time of this writing.


### `__name__`

`__name__` is assigned to the module/file that it is associated with. It can also be assigned to module, classes, functions, methods, descriptors or generator instances.


{% highlight python %}
# my_name_is_slim_shady.py
print(__name__) # will be set to `my_name_is_slim_shady`

class MyNameIsSlimShady(object):

    def say_my_name(self):
        print("My name is:")

{% endhighlight %}

Calling the __my_name_is_slim_shady.py__ from a file __what_is_my_name.py__ outputs `my_name_is_slim_shady` and class name `MyNameIsSlimShady`.

{% highlight python %}
# what_is_my_name.py

print("What's my name?")
import my_name_is_slim_shady

# Outputs:
# What's my name?
# my_name_is_slim_shady

ss = my_name_is_slim_shady.MyNameIsSlimShady()
ss.say_my_name()
print(ss.__class__.__name__)

# Outputs:
# My name is:
# MyNameIsSlimShady
{% endhighlight %}

### `__main__`

The `__name__` variable for the module that is being executed is set to `__main__` for a script, standard output or the interactive terminal. It is used to check if a module is invoked directly or called from a different file, see above description for [`__name__`](#__name__). 

When we run the file __my_name_is_slim_shady.py__ alone we see that `main_my_name_is_slim_shady()` function is invoked since the `__name__` for the file is set to `__main__` for this module.

{% highlight python %}
# my_name_is_slim_shady.py
def main_my_name_is_slim_shady():
    print("Standing Up!")

if __name__ == '__main__':
    main_my_name_is_slim_shady()

# Output:
# Standing Up!
{% endhighlight %}

When I import the __my_name_is_slim_shady.py__ file in __what_is_my_name.py__ it doesn't call the `main_my_name_is_slim_shady()` function but invokes `main_what_is_my_name()`.

{% highlight python %}
# what_is_my_name.py
import my_name_is_slim_shady

def main_what_is_my_name():
    print("Not Standing Up")

if __name__ == '__main__':
    main_what_is_my_name()

# Output:
# Not Standing Up
{% endhighlight %}

This idea can be use for larger projects to set a starting point of execution for the whole project. Like in older versions of [flask projects](http://flask.pocoo.org/snippets/20/) where you called `app.run()` from a python file.

If you like reading this post, please do share or leave your comments on the post, I would love get more feedback on my articles. And if you really like my articles do subscribe to my newsletter. I promise to not spam.

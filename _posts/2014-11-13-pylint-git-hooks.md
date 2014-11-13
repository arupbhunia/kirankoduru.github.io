---
layout: post
title:  "pylint and git hook for pre-commit"
date:   2014-11-13 00:40:00
categories: python
description : Pylinting your code even before it is committed
---

A short description of a git hook would be a script that runs when some important actions occur in your git repository. There are different kinds of actions (pre-commit, post-commit, pre-update, post-update, etc...) where you can run your code. What we will concentrate on would be the __pre-commit__ action.

A __pre-commit__ action is something that runs before you commit files into your git repository. This particularly is my favorite area for running pylint since, it stops me from committing code that doesn't follow the right python standards.

So first you need to install the following python packages from pip.

{% highlight bash %}
pip install pylint
pip install git-pylint-commit-hook
{% endhighlight %}

Next goto the git repository that you are working on and navigate to your __.git/hooks/__ directory. From the root directory of your project, run the following command to rename the __pre-commit.sample__ file to __pre-commit__

{% highlight bash %}
cd .git/hooks/
mv pre-commit.sample pre-commit
{% endhighlight %}

Delete everything thats in there and paste this in the __pre-commit__ file 
{% highlight bash %}
#!/bin/sh
git-pylint-commit-hook
{% endhighlight %}

What this does is, it checks for updates in all the __.py__ files from your last commit and runs [pylint](http://www.pylint.org/) on it. If your code doesn't pass the pylint rating then it won't let your commit your changes.

### Run pylint on `example.py`
{% highlight bash %}
pylint example.py
{% endhighlight %}

Fix the errors, warnings and suggestions you see from pylint. The git-pylint-commit-hook documentation can be found [here](http://git-pylint-commit-hook.readthedocs.org/en/latest/usage.html). It lets you also configure your __.pylintrc__ file in the git repository.

### Pylint on Sublime Text 2/3
Installing a pylint package that helps you validate your code while typing would be a good time saver. So from your __Sublime Text Package Control__ find a package that works best for you. If you don't have package control then here's a [link](https://sublime.wbond.net/installation) for you to download it.
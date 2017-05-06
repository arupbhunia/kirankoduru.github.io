---
layout: post
title:  "Pylinting with PyCharm"
date:   2017-05-06 15:03:15
categories: python
description : A how to guide on getting started with pylint on PyCharm 
author: Kiran Koduru
image: /img/pycharm-pylinter.jpg
---

If you have worked with PyCharm, you will have noticed the inspections plugin which performs static analysis on your code is very effective in finding PEP-8 errors. But it fails in some places and can be replaced by `pylint`. This tutorial will guide you through a step by step walkthrough of setting up `pylint` in PyCharm.


#### 1. Locate your `pylint` installation

To find out where is `pylint` on most unix OS you can type the following in your command line.

<pre>
$ which pylint
/usr/local/bin/pylint
</pre> 

If you don't have `pylint` installed then try the command abover after installing `pylint` via pip.

<pre>
$ pip install pylint
</pre>

#### 2. Open External tools in PyCharm

You can find the *External Tools* options from the 

1. File -> Settings
2. Typing *External Tools* in the search bar

or

1. Pressing `Ctrl` + `Alt` + `S` on your keyboard
2. Typing *External Tools* in the search bar  

![Step image 2](/img/pycharm/1.jpg)

You can read more about *External Tools* [here](https://www.jetbrains.com/help/pycharm/2017.1/external-tools.html).


#### 3. Setup External Tools

Click the `+` icon in the *External Tools* window and configure using the following information. Make sure that <b>Program</b> value is set to the output from [Step 1](#1-locate-your-pylint-installation).

![Step 3 image](/img/pycharm/2.jpg)

#### 4. Finally run `pylint`

Run `pylint` from *External Tools*  via *Tools -> External Tools -> pylint* dropdown.

![Step 4 image](/img/pycharm/3.jpg)

#### 5. View your output in PyCharm console

After your run from [Step 4](#4-finally-run-pylint), you can view your `pylint` score in your PyCharm console.

![Step 5 image](/img/pycharm/4.jpg)

Though this tutorial isn't a complete replacement for your inspections plugin, I hope this helps you get started with `pylint` on PyCharm. I would definitely like to hear your thoughts and opinions on how to pipe the output from our setup to PyCharm inspections plugin. 
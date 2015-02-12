---
layout: post
title:  "Setup git repository on your server"
date:   2015-02-11 20:38:09
categories: git
description : Setup git for your Linux server
---
As much as you like using [github](http://github.com) for your public repositories you need a way to host your codebase where you deploy your project sometimes.

You can bookmark this for future reference to get it running quickly. I usually work with ubuntu on AWS EC2 for setting up my projects. To be able to perform these you can use an Linux operating system servers and do it quite easily. So for the purpost of this example I am going to assume you are using EC2 systems too.

- __Step 1__: Login to your EC2 machine through ssh. For ubuntu users login using ssh

{% highlight bash %}
ssh ubuntu@[your-server-ip]
{% endhighlight %}

- __Step 2__: Once you are in. then you create a git bare repository. You create a directory first and then make it a bare repository. For the purpose of this blog I will call my project _unicorn_ and the bare git repository as __unicorn.git__.
{% highlight bash %}
mkdir unicorn.git
cd unicorn
git init --bare
{% endhighlight %}

- __Step 3__ : Create a place you want to export your project files to. Since the default path when I login in EC2 is `/home/ubuntu/`, I will create a project directory right there and call it __unicorn__.
{% highlight bash %}
mkdir unicorn
{% endhighlight %} 

- __Step 4__ : Once I am done I need to setup the git hook for whenever my project is updated. Which is called `post-update.sample` which should be in your __unicorn.git__ folder. We need to first rename the file to `post-update`.
{% highlight bash %}
cd unicorn.git
cd hooks/
mv post-update.sample post-update
{% endhighlight %}


- __Step 5__ : Clear everything in there and add this to the file. What it does, is export the git files to the directory `/home/ubuntu/unicorn/` . But if you have setup a different path for where you want to export your files then, do so.
{% highlight bash %}
#!/bin/sh
GIT_WORK_TREE=/home/ubuntu/unicorn/ git checkout -f
{% endhighlight %}

- __Step 6__ : Next logout from the server and goto your local git repository for project __unicorn__ and add a remote.
{% highlight bash %}
git remote add production ubuntu@[your-server-ip]:/home/ubuntu/unicorn.git
{% endhighlight %}

- __Step 7__ : That's it ! Now push __master__ branch to the remote __production__ and checkout the `/home/ubuntu/unicorn` directory for the files.
{% highlight bash %}
git push production master
{% endhighlight %}

Also if you would like to checkout how to write `pre-commit` files for __pylint__ing your code then checkout [this blog post](kirankoduru.github.io/python/pylint-git-hooks.html).

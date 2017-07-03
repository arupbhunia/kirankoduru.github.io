---
layout: post
title:  "Deploy python package with private github repo"
date:   2017-07-03 16:08:47
categories: python
description : Deploying python package with private github repository
author: Kiran Koduru
image: /img/create-new-github-access-token.jpg
---

If you are someone who hosts your python package in a private github repository for work or someone who plans to develop your package privately before it is open sourced, I have answers to your questions on how to deploy your project without hosting your own private PyPI service.

First, I hope you have followed the guide on how to distribute your package. If not, [this](https://packaging.python.org/tutorials/distributing-packages/) might come in handy. At most you have a __setup.py__ file in place for your package. Next you can follow these steps for a elaborate walk through.

#### 1. Create github user
Create a github user with access to this one private python repository. This is in case your access token in [Step 2](#2-create-a-github-access-token) is exploited, this step will mitigate any risk to your access token.

#### 2. Create a github access token
Login as user from [Step 1](#1-create-github-user) and [create a new access token](https://github.com/settings/tokens/new) with _Full control of private repositories_ as scope. This will allow to read, write and delete your private repo. Hence creating a new github user will minimize the risk to just this repo if your access token ever gets compromised.  

#### 3. Tag your git repository 
Next, tag and release your private github repository. Either use the [github UI](https://help.github.com/articles/creating-releases/) to tag your release or if you would like to use the command line

<pre>
git tag 0.1.0
git push origin --tags
</pre>

This steps allows you to make rolling deploys in case you don't want to deploy the updated projects all at once or have different versions in different projects.

#### 4. Add to requirements.txt 
Finally update your __requirements.txt__ file in the python project that depends on this private project and let `pip` take care of the rest of it. 

<pre>
git+https://{access_token}:x-oauth-basic@github.com/{username}/{project}.git@0.1.0
</pre>

What I mean is if your deployment practices follows installing packages with 

<pre>
pip install -r requirements.txt
</pre> 

then your project will update the package version with each deploy. Make sure you've replaced `{access_token}`, `{username}` and `{project}` appropriately for the command above.

---
layout: post
title:  "Write a CHANGELOG"
date:   2016-08-31 21:02:00
categories: software-engineering
description : You should write a CHANGELOG
---

Software engineering is not just about writing code, at times it includes documenting things that you never would have if it was your personal side project.

Working with a StartUp I realized that it's important to communicate to your users and your team members about the changes that go out with each release. Thus, slowly began my phase of tagging each release. I use [semver](http://semver.org/) for all of my projects but you could choose to use any of the plethora of versioning [schemes](https://en.wikipedia.org/wiki/Software_versioning#Schemes) out there.

Tagging releases is useful if you believe in continuous deployment. If a release fails, you can always roll back to the previous versions. Come back and fix it tomorrow! But the part that's most useful in helping identify what could possibly have broken your production environment could be hidden in your CHANGELOGs.

I use [git](https://git-scm.com/) for my projects so the obvious place to look for before writing any CHANGELOGs are my *git* commits. My workflow includes comparing differences between 2 releases and identifying things that are crucial to be included in a CHANGELOG. While writing CHANGELOGs is not fun I wanted to help fellow developers start off writing their own CHANGELOGs. Thus evolved my python package [changelog-suggest](https://pypi.python.org/pypi/changelog-suggest) from my experience from writing innumerable CHANGELOGs. 

It's a command line tool that goes through your git commit log history and echoes them onto your command prompt. Though it's not the CHANGELOG that you would want to put out there for your users, it does help you get started. If you are someone who includes your bug tracker issue numbers in your git commits then this tool accepts issue regular expressions to search in your commits.

So go on and write a CHANGELOG. It could be something you will reminisce some day.

---
layout: post
title:  "Happy New Year! A year in review"
date:   2017-01-02 22:25:40
categories: random
description : Reviewing my accomplishments for the year 2016
author: Kiran Koduru 
---
Welcome to the year 2017 everyone! I hope you had a good 2016. There are a lot of things that you might cherish about last year and some not so much but in the end, it is all about you being a better person than you were yesterday. 

On that same note, I wanted to reflect over some of the technical achievements and other non tech related achievements from 2016.

#### A year full of containers

I began my year with some [ansible](https://www.ansible.com/) but setting up 100 servers simultaneously was no joke. Thus [Docker](https://www.docker.com/) emerged as the weapon of choice. With the advent of container technology, my team and I quickly explored the area of container orchestration in the AWS cloud. We had a lot of resistance setting up but mostly thanks to [this awesome blogpost](http://www.ybrikman.com/writing/2015/11/11/running-docker-aws-ground-up/#creating-an-elb) (no seriously, bookmark this link) by Yevgeniy Brikman, we powered through setting up our [EC2 Container Service](https://aws.amazon.com/ecs/). We also recently transitioned some of our projects to google cloud and had to familiarize ourselves with [kubernetes](http://kubernetes.io/). Though both these container orchestration services massively lack in documentation, fiddling with them allowed us to scale our backend infrastructure much more easily.

#### https everything

A massive thanks to [letsencrypt](https://letsencrypt.org/) for providing the eternal service of encrypting everything on the web. A free certificate can mean a lot when running a small startup. I felt like I was abusing their service when I began to encrypt everything even on our staging environment, but some donations later, I don't feel so bad about it anymore.

#### Coverage for all

With at least 85% coverage on all my projects, I think I am now a major proponent of writing tests. When working with a dynamically typed language like python, stressing over testing every line of code is just as important. Even my [side](https://github.com/kirankoduru/arachne) [projects](https://github.com/kirankoduru/changelog) have over 95% test coverage. I may seem uptight about testing but it has allowed me to hand over code with much more confidence.

#### Monitoring

When one has over a 100 micro services running, it is only wise to monitor each service. With the help of [elasticsearch and kibana](https://www.elastic.co/products) a dashboard full of stats was assembled, allowing me to look closely at said services. Time series data can be easily crunched into elasticsearch and visualized in kibana with no regrets. Though a lot of code for monitoring was written from scratch, I learned my lesson to never waste time again building [bootstrap dashboards](https://wrapbootstrap.com/themes/admin) that confuse me on how many JS libraries I'll need to import to run just one visualization.   

#### And now some PHP

I crossed paths with my old friend, PHP, this year. I was responsible for maintaining my company's wordpress blog. The founder of the last startup I worked at told me, "The wordpress codex is all you need when developing in wordpress". And so I began reading the docs before writing my wordpress plugins.


#### Hackathons, meetups and conferences

I am getting too old to pull an all-nighter for a hackathon, but finally this year I demoed a fully finished project. I had the opportunity to attend [PyGotham](https://2016.pygotham.org/) this year and also be at Docker's 3rd anniversary meetup which was followed by some [Momofuku birthday cake](http://milkbarstore.com/main/menu/). Delicious!

#### 3 new countries

I finally got to see Canada, Mexico and Thailand this year. I spent over 4 weeks in vacation time this year. I am glad that I get to work with a team that is so supportive. Startups are tough, which makes me appreciate my time away. There are places I haven't been, maybe next year I will make it to Oktoberfest!

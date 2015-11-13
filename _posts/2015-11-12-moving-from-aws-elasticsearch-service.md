---
layout: post
title:  "Goodbye AWS elasticsearch service"
date:   2015-11-12 23:49:03
categories: python
description : Migrating from elasticsearch service by Amazon Web Services
---
On October 1st 2015, AWS launced a new service for elasticsearch. At work, we have a ton of AWS credits and from past experiences setting up elasticsearch they got my attention. 

But sadly those AWS credits could not be used against the elasticsearch service so after using this service for 1 month we had to move away and setup our own cluster. But first, here's what we learned from using the AWS elasticsearch service.

#### Elasticsearch version 1.5
As of today, elasticsearch version 2.0 is available and the AWS elasticsearch service is at 1.5. That's a big difference if you care about missing out on those bug fixes and shiny new feature releases.

#### IAM Policies
The only way you can possibly configure access to the Elasticsearch service is through the IAM policies. Though this is presumably a good way to secure inbound connections, there should have been a way to add security groups to the service. Also updating those IAM policies takes longer compared to changing security groups. 

#### No dynamic scripting
There is a good reason dynamic scripting is turned off by default in elasticsearch but if you ever wanted to turn this feature on, you cannot do it. You cannot upload scripts into the __scripts__ directory either if you wanted to practice dynamic scripting safely. 

#### You are not in control of your backups
You need to send out an email to AWS support asking them to send you a backup of your data. There is presently no other way to get access to the backups they preserve. If only they trusted the [elasticsearch-aws-cloud](https://github.com/elastic/elasticsearch-cloud-aws) plugin and allowed us to configure s3 repositories from the elasticsearch service dashboard. 

#### Autosharding
The amount of shards grew as we went from 1 million to 4 million records. I am unaware of how that feature works but it just did. [Someone said](http://blog.trifork.com/2014/01/07/elasticsearch-how-many-shards/) __sharding comes at a cost__, so now I am not sure if this is a good feature or a bad one.

#### No elasticsearch.yml access 
No, you are not in control of the __elasticsearch.yml__ file. So no settings to tweak. The elasticsearch service makes sure everything works perfectly.


What have your experiences been with the AWS elasticsearch service? Let me know in the comments below.
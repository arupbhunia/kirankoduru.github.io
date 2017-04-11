---
layout: post
title:  "elasticsearch and EC2"
date:   2014-10-30 01:00:00
updated: 2017-04-09 15:11:55
categories: elasticsearch
description : How we spent 3 hours trying to configure a 3 node cluster, and now I want to save you some time.
note: On 1 October, 2015 AWS released their elasticsearch service to make it easier to configure elasticsearch on EC2. I recommend checking it out.
---

Elasticsearch has taught me so much about text searching. It has been a love-hate relationship since I started using it. Though I still consider myself a newbie, but the struggle has taught me how crucial it is to index my data the right way. Mapping it correctly, using the right analyzers on the right fields. This post isn't about how great elasticsearch is but, about how there is so little to almost no documentation about setting up a elasticsearch cluster on EC2.

We started off with 3 m1.small EC2 instances from Amazon. We had been running elasticsearch v.1.2.1 before on these machines but hey ! there was a new upgrade. So we installed v1.3.x through [yum](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/setup-repositories.html#_yum). Once we had that going, we had to install the [elasticsearch-cloud-aws](https://github.com/elasticsearch/elasticsearch-cloud-aws) plugin. You can have to navigate to the installed elasticsearch directory and run these commands. We had v.1.3.x installed so we had to get the v2.3.0 plugin. You can checkout the github repo for the right version to use with your elasticsearch installation. 

<pre>
$ cd /usr/share/elasticsearch/
$ bin/plugin -install elasticsearch/elasticsearch-cloud-aws/2.4.0
</pre>

Next was to setup the `elasticsearch.yml` file. The config file can be usually found at `/etc/elasticsearch/elasticsearch.yml`. These are the settings we used for the master and slaves nodes were almost the same, except we had to set the value of the `node > master` to `true` or `false` accordingly. 

<pre>
cluster.name: marvel_universe

cloud:
    aws:
        region: us-east-1
        access_key: ACCESS_KEY_FOR_AWS
        secret_key: ACCESS_SECRET_FOR_AWS

name:
    name: hulk

node:
    master: true
    data: true

discovery:
    type: ec2
    zen:
        ping:
            multicast:
                enabled: true

    ec2:
        tag:
            clustername: marvel_universe
</pre>

Once we are done with the configurations we had to setup our security groups for the EC2 instances. This was the most crucial part for us to figure out since we cared about every port that we left open. Since we had launched our instances in a __VPC__ we only had a new different changes from instances that are not in __VPCs__. Here are the connections we used.

__Inbound connections__ for your security group


| Type          | Protocol  | Port Range  | Source  |
|:--------------|:----------:|:------------:|--------:|
|SSH| TCP|22|[your-ip-address]|
|HTTP| TCP|80|0.0.0.0|
|HTTPS| TCP|443|0.0.0.0|
|Custom TCP Rule| TCP|9200|[elasticsearch-security-group]|
|Custom TCP Rule| TCP|9300|[elasticsearch-security-group]|


__Outbound connections__ for your security group

| Type          | Protocol  | Port Range  | Source  |
|:--------------|:----------:|:------------:|--------:|
|HTTP| TCP|80|0.0.0.0|
|HTTPS| TCP|443|0.0.0.0|
|Custom TCP Rule| TCP|9200|[elasticsearch-security-group]|
|Custom TCP Rule| TCP|9300|[elasticsearch-security-group]|

Next just run your elasticsearch servers individually and they will be able to detect eachother.

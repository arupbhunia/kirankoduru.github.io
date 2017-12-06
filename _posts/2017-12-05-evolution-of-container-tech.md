---
layout: post
title:  "Evolution of containers"
date:   2017-12-05 03:39:31
categories: devops
description : The evolution of container technology
author: Kiran Koduru
image: /img/containerization.png
---
I know, I am a little late to chime in on this discussion on the evolution of containers but, lately I have been fascinated in learning how we started using [Docker](https://www.docker.com/) for software deployments. Surely there must have been other inputs in the creation of virtual environments. So I started reading, and went down a rabbit hole to find some answers.

In the beginning we ran batch processes, then we moved to timesharing, and finally years later, we found ways to share resources in a distributed computing world. Ever wonder how did we even get here? 

This post are my notes on the evolution of container technology and what led us to what is known today as, Docker.

### Virtualization

Creating inaccessible fortresses around compute resources has been something software developers have yearned for ages. In a hardware assisted virtualization environment you would provision a host OS on top of a guest virtual machine. This host OS allots resources to your guest OS programmatically. 

One of the pioneers in the world of hardware assisted virtualization is Amazon Web Services(AWS). AWS allows 2 types of virtualized environments for your EC2 instances, namely paravirtual(PV) and hardware virtual machine(HVM) instances. PV environments require a kernel modification which allows for hardware access to the host OS near native speeds. Whereas HVM environments, require a CPU that supports virtualization<sup><small>[1]</small></sup>. They both run on top of a [hypervisor](https://en.wikipedia.org/wiki/Hypervisor) that allows for creation of guest virtual machines. Since the rise in HVM technology in the recent years, AWS suggests on using HVM instances over PV when picking EC2 instances<sup><small>[2]</small></sup>.

What I would like to talk about today is, software virtualization. It's the use of the underlying libraries on the host OS that provide isolation of environments. I won't go highlighting every step in the history of containerization, but would love to walk through some important stages that have led to containers as we see them today. 

### chroot

The year was 1979. Version 7 of Unix was being developed and along with it came __chroot__. Initially, it was used to create virtualized copies of softwares to make building and testing easy, but was later included in the kernel as we see today. The __chroot__ system call allows to set the root directory for a given user. Obviously, it shouldn't be run on a super user, who can break out of a __chroot *jail*__ quite easily.

> An early use of the term "jail" as applied to chroot comes from Bill Cheswick creating a honeypot to monitor a cracker in 1991.
> 
> Checkwich, Bill (http://csrc.nist.gov/publications/secpubs/berferd.pdf)

### Linux VServer

Introduced in the year 2001, Linux VServer provided resource isolation for file system, CPU time, network addresses and memory for Linux instances running on the same physical machine<sup><small>[3]</small></sup>. Each of these resource partitions are called security contexts. It allowed for processes to not accesses anything outside their partition and was initially shipped with kernel patches that had VServer installed.

### Open Virtuozzo (Open VZ)

In 1999 Alexander Tormasov proposed an idea to Sergey Beloussov about containers as a set of processes with namespace isolation, file system to share code/RAM and resources<sup><small>[4]</small></sup>. Though the term __containers__ wasn't coined until 2005, they started to develop something container like into the Linux kernel around the year 2000. It was not until 2002, that they released a version for the Linux operating system.

OpenVZ used a single patched Linux kernel where guest and the host OS shared the same kernel versions. Something that could be considered a drawback, if you wanted to work with different versions of the kernel.

### Solaris Containers/Zones

Sun Microsystems(now Oracle) introduced Solaris Zones in the year 2005 in Solaris 10. It provided a completely isolated virtual server in a single OS instance. Each zone as they were known, was an isolated execution environment. Even though each application shared the same operating system, it seemed like they were running on their own machine. These containers were light weight since they separated physical hardware with logical server management<sup><small>[5]</small></sup>. It was also called __chroot on steroids__ in the later years.

### cgroups

While working for Google, Paul Menage and Rohit Seth developed what was then called process containers in the year 2006. They renamed it to __cgroups (control groups)__ in late 2007 to avoid confusion with the term __container__. In their implementation, processes were grouped together and those groups shared memory, CPU and disk IO. Soon after, __cgroups__ was merged into the Linux kernel on January 2008. 

If you would like to implement your own __cgroups__ then [this video](https://sysadmincasts.com/episodes/14-introduction-to-linux-control-groups-cgroups) by Justin Weissig is a great starting point. 

### Linux containers - LXC

LXC was created by a group of engineers at IBM around 2008. It works as a combination of __cgroups__ and isolated namespaces<sup><small>[6]</small></sup>. It leverages the internal kernel API for namespaces, __chroot__, __cgroups__ etc. for container process creation. The goal of Linux containers has been to allow running standard linux installations without the added kernel dependency<sup><small>[8]</small></sup>.

If you would like to start building your own Linux containers then here's a link to the [getting started guide](https://help.ubuntu.com/lts/serverguide/lxc.html).

### Docker

In the year 2013 Docker made it's debut at my favorite conference, PyCon. Docker's founder Solomon Hykes gave a lighting talk at PyCon while working for dotCloud. Docker, again makes use of isolated namespaces to provision processes, networking and filesystem resources. Docker used __lxc__ as the execution driver in the past to make calls to the Linux kernel's internal API but __lxc__ was later replaced by libcontainer<sup><small>[7]</small></sup> which frees __Docker__ from any dependencies in it's new releases. 

That's it for now but I'd like to add that, there's been progress in the container world past Docker. Hope this has been useful to you as much as it has been for me. I have also added reference links below, do check them out to learn more. Containerization could be the next step in allowing you to help build the next hosting company, and I hope I have helped you push you in that direction.

[1] [Hardware Virtual Machine (HVM) and Paravirtualization (PV)](https://www.linux.org/threads/hardware-virtual-machine-hvm-and-paravirtualization-pv.12475/)<br/>
[2] [AWS Linux AMI Virtualization Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html)<br/>
[3] [Linux VServer Changelogs Version 0.0](https://web.archive.org/web/20110705084600/http://www.solucorp.qc.ca/changes.hc?projet=vserver&version=all)<br/>
[4] [Open VZ History](https://openvz.org/History#1999)<br/>
[5] [High Availability and Disaster Recovery: Concepts, Design, Implementation - Klaus Schmidt](https://books.google.com/books?id=adU_AAAAQBAJ&lpg=PA186&dq=%22chroot%20on%20steroids%22&pg=PA186#v=onepage&q=%22chroot%20on%20steroids%22&f=false)<br/>
[6] [Wikipedia - LXC](https://en.wikipedia.org/wiki/LXC)<br/>
[7] [Introducing execution drivers and libcontainer](https://blog.docker.com/2014/03/docker-0-9-introducing-execution-drivers-and-libcontainer/)<br/>
[8] [LXC Features](https://linuxcontainers.org/lxc/introduction/#features)<br/>
[Illustration courtesy Vecteezy](https://www.vecteezy.com/)
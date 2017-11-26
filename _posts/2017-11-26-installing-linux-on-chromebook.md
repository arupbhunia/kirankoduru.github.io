---
layout: post
title:  "Installing linux on my chromebook - The Acer C740"
date:   2017-11-26 12:22:00
categories: random
description : Steps to install linux on the chromebook - Acer C740
author: Kiran Koduru
image: /img/gallium-os.png
---
Happy Thanksgiving everyone! For this Black Friday I purchased a [new chromebook](http://www.microcenter.com/product/487664/C740-C9QX_116_Chromebook_-_Gray) from Acer for $149. I have been looking for something in a light weight laptop department. This one weighs less than 3 lbs and lasts for 8 hours on a single charge. I was more than happy to carry a basic chromebook than a bulky laptop. The only issue was running something other than __ChromeOS__ since I wanted to do more than just browse the internet. 

I wanted to choose a flavor of linux that was easy to install. [crouton](https://github.com/dnschneid/crouton) and [gallium](https://wiki.galliumos.org/) seemed to be two good choices for installation. I went with __GalliumOS__ since it had good community support and better hardware compatibility. 

Gallium is also easy to install. Once you [prepare your chromebook](https://wiki.galliumos.org/Installing/Preparing), you can install it via a bootable USB/SD card or use [chrx](https://chrx.org/) script to dual boot alongside __ChromeOS__. I went with __chrx__ which is as simple as running a `curl` [command](https://wiki.galliumos.org/Installing#chrx_Installation).

And finally I can dual boot with `Ctrl + L` to boot to __GalliumOS__ screen or `Ctrl + D` to boot to __ChromeOS__. Either way, this chromebook is now good for writing small python applications or writing blog posts. The only qualm I have is that I wish it had more SSD capacity, 25 GB doesn't cut it if I am working with a lot of data. But I could always expand it in the future. 

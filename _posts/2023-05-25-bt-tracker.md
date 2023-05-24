---
layout: post
title: Interesting details on Bluetooth trackers
tag: privacy
author: Olivier Bonaventure
---

We are clearly moving to the Internet of Things with more and more devices equipped with network interfaces. While Bluetooth was initially designed to connect small devices like earphones or microphones to smartphones, its low energy consumption has enabled new use cases. One of these use cases are the tracking tags such Apple's [AirTag](https://www.apple.com/airtag/) or [Tiles](https://be.tile.com/en).

Thanks to these tags, anyone can find the current location of any object to which the tag is attached. This geolocation leverages all the deployed smartphones that listen to the Bluetooth emitted by these tags and report the tags that they heard to online services. Your smartphone probably contributes to the geolocation of tags placed by other users.

Erik Rescorla wrote a very interesting blog post on [Defending against Bluetooth tracker abuse: it's complicated](https://educatedguesswork.org/posts/unwanted-tracking/). This post describes the operation of these Bluetooth tags and discusses some of the privacy implications of their deployment. A must read !




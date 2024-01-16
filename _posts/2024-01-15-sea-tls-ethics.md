---
layout: post
title: The cloud under the sea, BGP growths slower, ethics and testing TLS
tag: optical fiber, BGP, ethics, TLS
author: Olivier Bonaventure
---

Internet backbones rely mainly on optical fiber links. ABC published a very interesting documentary entitled [the cloud under the sea](https://www.abc.net.au/news/2023-12-18/undersea-internet-cables-a-hotspot-for-espionage/103240612) that describes how these optical fibers are placed in our oceans and all the different techniques that are used to deploy and operate these important networks.

Every year, Geoff Huston publishes a detailed post blog that analyzes the growth of the BGP routing tables based on the data that he collects in Australia. For many years, he has been observing a continuous growth of the BGP IPv4 routing table and tried to explain the different factors that influence this growth. In 2023, he observed the beginning of a plateau in the growth the BGP IPv4 routing tables. Year 2024, will tell us whether the BGP routing tables will reach 1 million entries or start to decline.


![BGP table 2016-2024]({{site.baseurl}}/images/bgp-table-2023.png)


The web plays a very important role in our digital society and some of the technical decisions that network engineers take could have a potential impact on the entire society. The [W3 consortium](https://www.w3.org) has started to work on a document on [ethical web principles](https://www.w3.org/TR/ethical-web-principles/). This document aims at setting the principles that will drive the efforts of the [W3 consortium](https:/www.w3.org) but are useful for many web professionals as well.

[Transport Layer Security](https://beta.computer-networking.info/syllabus/default/protocols/tls.html) is used by many applications and servers. It can be difficult to completely configure TLS servers. The [testssl](https://testssl.sh/) project provides a very convenient shell script that performs a large number of tests on TLS servers.



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
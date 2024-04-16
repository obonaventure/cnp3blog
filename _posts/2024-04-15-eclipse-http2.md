---
layout: post
title: A solar eclipse influences Internet traffic, reverse traceroute,  xzutils backdoor, page load times, http/2 attacks using continuation frames, Gigabit Ethernet links going down
tag: measurements, traceroute, security, web
author: Olivier Bonaventure
---

Last week, a solar eclipse was visible in North America. Amazon engineers looked at some of the traffic data they collect to see whether their Internet traffic was affected by the solar eclipse. They expected an indirect effect as humans were looking at the eclipse instead of using the Internet. They observed a [visible effect](https://www.amazon.science/blog/using-amazon-web-traffic-to-track-the-eclipse) by looking at the traffic decrease per groups of post codes every ten times. In case of doubt, this is another illustration of the massive amount of data that Amazon collects and uses...

![eclipse]({{site.baseurl}}/images/eclipse-amazon.png)

[Kevin Vermeulen](https://kvermeul.github.io) provides [a brief explanation of reverse traceroute](https://pulse.internetsociety.org/blog/reverse-traceroutes-help-troubleshoot-improve-visibility-of-internets-health) and its inclusion in the [Network Diagnostic Tool](https://www.measurementlab.net/tests/ndt/) maintained by [measurement lab](https://www.measurementlab.net). 

Last week, a backdoor in the widely used xzutils package was detected before the inclusion of the latest version of this software in popular Linux distributions. If successful, this could have affected the security of many ssh servers. [Bruce Schneier](https://www.schneier.com/) provides a concise description of this problem on his [blog](https://www.schneier.com/blog/archives/2024/04/backdoor-in-xz-utils-that-almost-happened.html). 

There are also ongoing attacks that affect different implementations of HTTP/2. HTTP/2 is a binary protocol that contains several types of frames. A new type of attack described in details in a [blog post](https://nowotarski.info/http2-continuation-flood-technical-details/) abuses the HTTP/2 CONTINUATION frames to cause memory exhaustion on servers.

DebugBear analyzes [the time to load web pages on many websites](https://www.debugbear.com/blog/page-speed-2024) by leveraging the [Google Chrome CrUX dataset](https://www.debugbear.com/blog/chrome-user-experience-report that collects speed information from many real chrome users. The average page load time is 1.3 seconds, with desktop being slightly faster than mobile. During the last the page load time has decreased slowly.

![page load times]({{site.baseurl}}/images/debugbear-plt.png)


Daniel Dib continues his exploration of Gigabit Ethernet. In his [latest blog post](https://lostintransit.se/2024/04/10/1000base-t-part-4-link-down-detection/?doing_wp_cron=1713273138.0107200145721435546875), he explains how network cards detect a link to be down. This is important for failure detection and fast recovery.


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
---
layout: post
title: The BGP horror show
tag: bgp
author: Olivier Bonaventure
---

The [Border Gateway Protocol (BGP)](https://beta.computer-networking.info/syllabus/default/protocols/bgp.html) is probably the most important Internet routing protocol. It is used by more than 80k Internet Service Providers and enterprise networks of various sizes to exchange routing information. BGP enables all Internet routers to obtain routes to reach Internet destinations.


BGP relies on a lot of manual configurations and a small error made by one network operator can have a huge impact on Internet routing. In a [recent blog post](https://www.kentik.com/blog/a-brief-history-of-the-internets-biggest-bgp-incidents/), Doug Madory summarized some of the important incidents that have affected BGP during the last decades.

The main incidents discussed in this post are: 

 - the AS7007 incident in 1997 where a router announced a large part of the IP addressing space as originating from AS7007. Similar incidents occurred in 2004, 2010 or 2015
 - the hijack of the IP prefix containing the DNS servers of Youtube in 2008 by the state of Pakistan and PTCL
 - man-in-the middle attacks proposed in 2008 and realized in 2013
 - routes leaked by different providers
 - attacks on cryptocurrency services
 
 This blog post is a nice starting point for students wishing to explore the different types of BGP incidents.
 
 You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 
 

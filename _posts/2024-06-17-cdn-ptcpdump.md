---
layout: post
title: Mapping CDNs, Configuring SSL/TLS, ptcpdump and packet trips
tag: cdn, tcpdump, tls, traceroute
author: Olivier Bonaventure
---

Most of the content sent on the Internet these days is served by CDN servers from companies like Akamai, Netflix, Meta, Google, ... Steve Song has published a [nice map](https://opentelecomdata.org/cdns/) showing the location of these CDN servers. Furthermore, the [data](https://github.com/stevesong/cloud_cdn_cache/) is also available.

![Open telecom data of CDN sites]({{site.baseurl}}/images/Otdata.png)

TLS is the default on popular web servers. If you manage a web server, there are different configuration commands that you need to use to correctly enable TLS on your server. Mozilla"s [SSL Configuration generator](https://ssl-config.mozilla.org/#server=apache&version=2.4.41&config=modern&openssl=1.1.1k&guideline=5.7) provides the configuration snippets that are required by 22 different servers and load balancers.


![Mozilla SSL configurator]({{site.baseurl}}/images/Mozilla-ssl.png)

When students use [tcpdump](https://www.tcpdump.org) to capture packets on their
laptops, they often see packets corresponding to multiple processes, including
some running in the background that they often ignore. This creates some form
of "packet pollution" that makes it difficult for them to answer questions such
as "What are the packets exchanged between a given application and cloud servers ?". Fortunately, there are now solutions to this problem. On MacOS, Apple
ships a specific version of tcpdump that provides process information.


![MacOS tcpdump]({{site.baseurl}}/images/mac-tcpdump.png)

This is also possible on Linux using [ptcpdump](https://github.com/mozillazg/ptcpdump) developed by mozilla. ptcpdump uses eBPF to capture per process
information and attach it to packets. This requires a Linux kernel more recent than
5.2.

Networking students are familiar with traceroute when debugging networking problems. Traceroute is a basic command line tool, but there are alternatives that also probe the network to discover the path followed by packets. [trippy](https://trippy.cli.rs) is a nice alternative to traceroute. It supports various forms of traceroute with different protocols and can probe equal cost paths. Furthermore, it provides a text-based user interface that gives a lot of information about the paths being probed. 

![trip psg.com]({{site.baseurl}}/images/trip.png)




This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
---
layout: post
title: BGP errors, DMARC, video streaming, network design and IPv6 only networks 
tag: bgp, email, streaming, ipv6
author: Olivier Bonaventure
---


The [Border Gateway Protocol](https://beta.computer-networking.info/syllabus/default/protocols/bgp.html) is the most important routing protocol. BGP routers exchange routing messages over BGP sessions. These BGP sessions run on TCP connections between routers. An important characteristic of the BGP protocol is that since it relies on incremental updates, if a router detects a problem in an incoming message, it must tear down the corresponding session and thus discard all the routes learned over this session. This BGP session is usually quickly restarted and routes are reannounced.

The worst case for BGP is when a router sends an erroneous message over a BGP session and resends the same error when the session restarts. BGP implementations take great care to avoid sending errored messages to prevent this situation. However, on the receive side, Ben Cartwright-Cox reveals on a [blog post](https://blog.benjojo.co.uk/post/bgp-path-attributes-grave-error-handling) that several router vendors do not correctly process incoming erroneous messages.

Modern [electronic mail](https://beta.computer-networking.info/syllabus/default/protocols/email.html) relies on a variety of protocols and requires a lot of tuning by system administrators willing to configure their own email servers instead of relying on the large cloud platforms. [https://www.learndmarc.com/](https://www.learndmarc.com/) provides an interactive illustration of many of these different protocols. You simply need to send an email from your own domain to observe how it is handled.

[Pim van Pelt](https://ch.linkedin.com/in/pim-van-pelt-474466263) provides a detailed description of the design and the configuration of the network behind [AS8298](https://ipinfo.io/AS8298). There are few detailed descriptions of deployed networks and this is always useful for students. We could follow the [video](https://video.ipng.ch/w/omK5krK7jUPjfX6jG99BKA) or read the [slides](https://docs.google.com/presentation/d/1G6lzMrLYqno5BLiW70z-22ZZXPIDrAh-Z-BjedeZNZ0/mobilepresent?slide=id.gc6fa3c898_0_0).

Some network operators have started to deploy IPv6 only networks, but as some web sites and services are only available over IPv4, they need to provide a solution to enable access to these legacy services. Pavel Odintsov details in a [blog post](https://pavel.network/building-gateway-to-access-legacy-ipv4-internet-from-ipv6-only-work-laptop/) how to configure such a gateway to ensure that your IPv6 only laptop can reach IPv4 only websites.

Live video streaming consumes a growing fraction of the Internet bandwidth. A recent survey, entitled [Toward One-Second Latency: Evolution of Live Media Streaming](https://arxiv.org/abs/2310.03256) describes the evolution of the protocols used by major streaming services. This survey provides a good summary and plenty of references for those willing to explore this interesting topic. 


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 

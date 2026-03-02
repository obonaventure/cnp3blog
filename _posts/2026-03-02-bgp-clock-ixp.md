---
layout: post
title: Networking notes for March 2026
tag: speedtest, starlink, ospf, is-is, telnet, bgp, ip geolocation, IXP, BBR
author: Olivier Bonaventure
---

Mike Dano provides [an interesting analysis](https://www.ookla.com/articles/2025-global-satellite-broadband-performance-report) of the deployment and performance of satellite based Internet access networks (mainly Starlink) based on Speedtest measurements.

Bruce Davie discusses [the potential impact of improved shortest path computation algorithms](https://systemsapproach.org/2026/02/09/faster-than-dijkstra/) on link state routing protocols such as OSPF and IS-IS.

Ivan Pepeljnak provides [interesting information about the performance of routers thirty years ago](https://blog.ipspace.net/2026/02/mpls-forwarding-performance/) and how they shaped some of the design of MPLS.

Since the early days of the Internet, [telnet](https://www.rfc-editor.org/rfc/rfc854) has been a very popular method to access remote servers. It has been slowly replaced by ssh since the late 1990s. Recently, a severe flaw has been identified in one of the most popular implementations of telnet that is deployed on a wide range of embedded devices. A [recent study](https://www.labs.greynoise.io/grimoire/2026-02-10-telnet-falls-silent/) shows that multiple ISPs have started to block TCP on port 23 to prevent exploitation of this vulnerability.

An interesting [NANOG presentation](https://nanog.org/events/nanog-96/content/5574/) describes the BGP architecture of the Netflix network.

The [BGP clock](https://nanog.org/events/nanog-96/content/5599/) is an interesting experiment that allows detection of BGP zombies on the Internet. A BGP zombie is an IP prefix that has been withdrawn but whose route is still present in some routers. 

Geoff Huston provides his [annual review of the evolution of the global BGP routing tables](https://nanog.org/events/nanog-96/content/5629/). 

A free IP geolocation database is available from [https://ip66.dev/](https://ip66.dev/).

[Arelion](https://www.arelion.com), aka [AS1299](https://lg.twelve99.net), one of the international Tier-1 ISPs, provides [an interesting graph](https://blog.arelion.com/2026/02/17/a-connectivity-milestone-200-tb-s/) on the evolution of the traffic that it carries. 

Mike Nottingham wrote an [interesting opinion](https://www.mnot.net/blog/2026/02/20/open_systems) explaining why the Internet is and should remain an open system.

Internet exchange Points (IXP) are a key component of the Internet as they allow ISPs to peer at different locations. Tommaso Caiazzi shows how to build a [digital twin](https://labs.ripe.net/author/tcaiazzi/testing-ixp-configurations-without-breaking-production-a-digital-twin-approach/) to reproduce such an IXP using [Kathara](https://www.kathara.org/).

The BBR congestion control algorithm is replacing the CUBIC algorithm in some TCP implementations. A recent [survey](https://dl.acm.org/doi/10.1145/3793537) (36 pages!) summarizes the literature on this congestion control scheme and provides some measurement results.

Mingwei Zhang and Bryton Herdes provide a nice description of the [Autonomous System Provide Authorization (ASPA)](https://blog.cloudflare.com/aspa-secure-internet/) that extends the RPKI to secure interdomain routing.

Some git repositories are [being overloaded](https://www.theregister.com/2026/02/28/open_source_opinion/) by companies that create thousands of git pulls per day due to misconfiguration or bad practices.


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
---
layout: post
title: Networking notes for May 2026
tag: ripe atlas, traceroute, tcpdump, traffic, optics, time, dns, wireless, tls, resilience, http, linux
author: Olivier Bonaventure
---

The [RIPE Atlas](https://atlas.ripe.net) is an open measurement platform that collects measurement data (ping, traceroute, …) about Internet performance throughout the world. 
Yevheniya Nosyk describes in a [blog post](https://labs.ripe.net/author/yevheniya-nosyk/a-day-in-the-life-of-ripe-atlas/) what happens on a typical day on this platform.

The [Calypso project](https://www.calypso.voyage) leverages traceroute data to provide detailed maps of the submarine cables.

[Julia Evans](https://jvns.ca) has contributed new examples to the [tcpdump manpage](https://www.tcpdump.org/manpages/tcpdump.1.html#lbAF).

Flavio Luciani shares traffic graphs from an Indonesian IXP showing how [Internet traffic changes during Ramadan](https://www.linkedin.com/feed/update/urn:li:activity:7430541632862773248/).

In a series of [posts on Mastodon](https://social.afront.org/@kwf/109492743645650432), Kenneth Finnegan describes in detail what’s inside a 100 Gbps LR4 optic. Interesting read if you’d like to understand what’s inside these « connectors ».

Geoff Huston provides a nice explanation on [how time is managed on the Internet](https://www.potaroo.net/ispcol/2026-03/nts.html). 

A [nice visualisation](https://mxmap.ch/) based on DNS MX records shows where email of 2,100 Swiss municipalities are hosted. 

In a [short article](https://spectrum.ieee.org/telecom-history-1g-to-6g) published in IEEE Spectrum, Malik Tatipamula and Vint Cerf describe the last 40 Years of Wireless Evolution in cellular networks and the evolution from 1G to the forthcoming 6G. 

The [IPv6 over nothing IETF draft](https://ipv6tao.blogspot.com/2026/03/ipv6-over-nothing-internet-draft.html) proposes to use IPv6 without an underlying datalink layer.

TLS Encrypted Client Hello is now officially published as [RFC9849](https://www.rfc-editor.org/rfc/rfc9849.html). Samuel Henrique explains in a [short blog post](https://samueloph.dev/blog/i-use-curl-with-ech-btw-in-debian/) how to use ECH with curl on Debian.

In a [short blog post](https://www.mnot.net/blog/2026/03/25/using_ai), Mark Nottingham explains how he used Notebook LLM to analyze the progress of IETF working groups. 

Another demonstration of the importance of network connectivity. In Nordic countries, a large fractions of the payments in shops are done online. To cope with the risks of massive network disruptions, a new [law](https://www.reuters.com/business/finance/nordics-estonia-plan-offline-card-payment-back-up-if-internet-cut-2025-05-07/) enables offline payments in case of major network disruptions.

A set of [Claude skills](https://github.com/lugasia/3gpp-skill) to explore the 3GPP specifications, from 2G to 6G. 

An interesting [blog post](https://austinsnerdythings.com/2026/04/26/ptp-osa5401-26-nanoseconds-raspberry-pi/) showing how a cheap NTP server evolved over time to obtain 26 nanosecond clock accuracy.

Linus Torvalds has started to [remove old networking code](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=64edfa65062dc4509ba75978116b2f6d392346f5 ) from the Linux kernel. This removal affects protocols such as AX.25 (amateur radio), ATM, and old Ethernet drivers.  

Reflections on [the implementation of the Carrier Pigeon Internet Protocol](https://nxdomain.no/~peter/rfc1149_implementation_25_years_later.html), RFC1149, 25 years later


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
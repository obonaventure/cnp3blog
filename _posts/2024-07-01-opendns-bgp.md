---
layout: post
title: BGP routing policies, openDNS blocked, DNS history
tag: bgp
author: Olivier Bonaventure
---

The global Internet interconnects about 80,000 ASes through peerings with two main types of
routing policies: customer-provider and share cost. In a [blog post](https://labs.ripe.net/author/savvas-kastanakis/20-years-of-inferring-interdomain-routing-policies/), Savvas Kastanakis presents the
evolution of the Internet and its routing policies during the last twenty years. In 1998, the
Internet gathered 3,549 ASes with 6,475 links. 13% of these links with shared cost peering links.
In 2023, 75,160 ASes had 494,508 links with 69% of shared cost peering links. The blog post also highlight several other findings of the study such as the importance of selective announcements.

The [OpendDNS](https://www.opendns.com) open DNS resolver is [not available anymore in France and Portugal](https://support.opendns.com/hc/en-us/articles/27951404269204-OpenDNS-Service-Not-Available-To-Users-In-France-and-Portugal). This is not because of technical problems, but due to requests from judges. In France, several companies have complained about web sites that provide access to live sport events. The court has ruled that DNS resolvers should block the access to these offending domain names. Apparently, OpenDNS preferred to completely disable the entire service instead of implementing the required per-country filters. There is some additional information [in French](https://twitter.com/reesmarc/status/1806740386880082203). This has caused collateral damages as some IoT devices are configured to use OpenDNS by default...



On the [APNIC blog](https://blog.apnic.net/), Geoff Huston provides a brief but interesting [summary of the evolution of the DNS during the last forty years](https://blog.apnic.net/2024/07/01/dns-evolution/). He points to the key RFCs that document the evolution of the Domain Name System. Interestingly, he spends more space on discussing the recent approaches to improve the privacy of DNS queries using DNS over TLS, HTTPS, QUIC, ... than on DNSSEC that focused on improving the authenticity of the DNS responses. 




This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
---
layout: post
title: Networking notes for November 2026
tag: history, browser, dnssec, udp, socket, ecdsa, dns, smt, rpki,  
author: Olivier Bonaventure
---

The [Nordstjernen browser](https://www.nordstjernen.org/) is a new browser written in C that relies on [curl](https://curl.se) to implement network protocols.

An [analysis of the first 10,050 RFCs](https://medium.com/@js_9757/what-10-000-rfcs-reveal-about-58-years-of-internet-history-3db9ecfd12ac) with a look at the most prolific authors, their geographical distribution, … Some interesting data.

The new [release](https://github.com/secdev/scapy/releases/tag/v2.8.0) (2.8.0) of the scapy python based packet manipulation has been published.

Christies is selling a [printed copy](https://onlineonly.christies.com/s/fine-printed-books-manuscripts-science/carrier-pigeon-internet-protocol-150/325216) of one of the packets sent by the Bergen Linux Users group when they tested the first implementation of [RFC1149](https://www.rfc-editor.org/info/rfc1149/), “IP over avian carriers”. 

Barbara Jantzen provides on [circleid.com](https://circleid.com/) a very [detailed discussion on DNSSEC](https://circleid.com/posts/common-beliefs-about-dnssec-put-to-the-test-an-invitation-to-fact-check-your-assumptions). 

Martin Duke explains in [RFC10055](https://www.rfc-editor.org/info/rfc10055 ) the various techniques which need to be used on different operating systems to use Explicit Congestion Notification with UDP.

Anthropic’s Claude was used to explore the [origin of some of the parameters of the ECDSA cryptographic algorithm](https://www.anthropic.com/research/ecdsa-curve-origins).

Allison Marsh explains in [an IEEE Spectrum article](https://spectrum.ieee.org/bloomberg-terminal) the evolution of the Bloomberg terminal that allowed faster access to information about financial markets. For more than two centuries, finance has always tried to reduce the delay to distribute information.

On October 11th, the root DNS key was updated. Geoff Huston discusses this process in a [blog post](https://ispcol.potaroo.net/2026-10/kskroll.html). Jan Piet Mens also discusses the [key update](https://jpmens.net/2026/10/14/root-dnssec-trust-anchors/).

Job Snijders provides a [brief overview](https://blog.qrator.net/en/ASPA-what-to-expect_229/) of the Autonomous Systems Provider Authorization (ASPA) which allows ASes to securely announce their upstream providers so other ASes can validate received AS Paths using the RPKI. OpenBSD comes with all the required tools to implement ASPA.

The private keys of eight eMudhra Certification Authority certificates have been compromised. The eMudhra CA used by mistake [short private keys](https://www.bleepingcomputer.com/news/security/mozilla-google-apple-block-compromised-emudhra-ca-certificates/) which can be trivially recovered. Browser vendors and the TLS ecosystems have [reacted](https://emudhra.we-have-a-problem.com/). Scott Helm provides more details on the keys in a [blog post](https://scotthelme.co.uk/eight-bits-of-trust-emudhras-ca-keys-cracked-in-milliseconds/).

Version 1.0 of the modern Stalwart mail server has been released. It provides a significant [performance improvement](https://stalw.art/blog/stalwart-1-0/) compared to other popular mail servers.

SIDN Labs publishes [statistics updated daily](https://www.sidnlabs.nl/en/news-and-blogs/sidn-publishes-rssac002-traffic-metrics-for-nl) showing the DNS requests processed by the .nl nameservers. A nice observation point for the public DNS.

The [atlaspeering.com](https://www.atlaspeering.com/) website provides a [nice visualization](https://www.atlaspeering.com/public-peering-2026-top5-ranking/) of the evolution of the peering between ISPs on different IXPs.



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.

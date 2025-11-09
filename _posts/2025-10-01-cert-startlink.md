---
layout: post
title: Networking notes for October 2025
tag: certificates, Starlink, outages, DDoS, Wi-Fi 7, web, modem, email


author: Olivier Bonaventure
---

Cloudflare operates the 1.1.1.1 open DNS resolver. This resolver also supports DNS over HTTPS to protect name resolution from interception. When these protocols are used, the DNS client uses a TLS certificate for the 1.1.1.1 address to verify that it interacts with the right 1.1.1.1 server. Since the 1.1.1.1 address belongs to Cloudflare, this certificate should have been assigned to Cloudflare. However, in a [blog post](https://blog.cloudflare.com/unauthorized-issuance-of-certificates-for-1-1-1-1/), Cloudflare reveals that they have detected that the Fine ÇA certification authority has issued multiple certificates for 1.1.1.1 during the last year. Even if these certificates were apparently mainly used for testing, this questions the validation process used by some certification authorities…

The BurningMan festival attracts more than 70,000 attendees during 9 days in the Nevada desert. A growing number of these attendees bring Internet connectivity with them. An [Ookla study](https://www.ookla.com/articles/starlink-slows-down-during-burning-man) reveals that this year the festival attendees had a significant impact on the performance of Starlink in the region.

Several cuts affected optical fibres in the Red Sea recently. Doug Madory analyzes in a [blog post](https://www.kentik.com/blog/subsea-cables-parted-in-red-sea-again/) the impact of the failures on latencies in several cloud services and also on BGP routing.

Distributed denial of service attacks continue to grow, unfortunately. In early September, [an attack reaching 1.5 billion packets per second](https://fastnetmon.com/2025/09/09/press-release-fastnetmon-detects-a-record-scale-ddos-attack/) coming from IoT devices and CPE in 11,000 different networks was detected. Later, Cloudflare announced a new record for DDoS. They suffered an attack that [peaked at 22.2 Tbps and 10.6 billion packets per second](https://www.linkedin.com/posts/cloudflare_cloudflare-just-autonomously-blocked-hyper-volumetric-activity-7376009719557210112-I8Gd?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAA_wFcBtrzDYok3TgEsdsGk9Lnn1hU_xlI) but lasted only 40 seconds.

Linux powers most Internet servers, Android smartphones, and many IoT devices. Jonathan Corbet, who leads [LWN.net](https://www.lwn.net), provides a nice overview of the evolution of Linux and its ecosystem during the last three decades in a [one-hour-long video](https://m.youtube.com/watch?v=LS2LGMRzE1I) containing lots of interesting details.

In an interesting [article](https://spectrum.ieee.org/6g-bandwidth) published by [IEEE Spectrum](https://spectrum.ieee.org/), William Webb questions the quest for more and more bandwidth in access networks. The new generation of cellular technologies has brought higher bandwidth with each generation. Will we need even more bandwidth for 6G?

Today, cellular networks allocate one IP address to each smartphone. This will probably change in the coming months/years as Lorenzo Colitti and Patrick Rohr [have announced](https://android-developers.googleblog.com/2025/09/simplifying-advanced-networking-with.html) that Android 11+ will include support for DHCPv6 prefix delegation. This will enable network providers to delegate a /64 IPv6 prefix to each smartphone, which will be able to act as a real router and serve other devices connected using Wi-Fi or other wireless technologies such as 802.15.4.

At the end of September 2025, America Online (AOL) has [stopped its dial-up service](https://tedium.co/2025/08/11/aol-dial-up-ending-retrospective/). AOL was a giant during the Internet bubble in the late 1990s and was later replaced by ISPs and telecommunication providers offering various Internet access solutions. An IEEE Spectrum [article](https://tedium.co/2025/08/11/aol-dial-up-ending-retrospective/) summarises the last years of this service and reminds the young students of the era of dial-up modems.


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
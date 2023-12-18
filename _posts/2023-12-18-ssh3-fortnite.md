---
layout: post
title: Doom and L4S
tag: games, broadcast, latency
author: Olivier Bonaventure
---

[SSH](https://beta.computer-networking.info/syllabus/default/protocols/ssh.html?highlight=ssh#the-secure-shell-ssh) is the standard method to access remote servers and configure network devices. SSH runs above TCP and can forward TCP connections. François Michel has explored how secure shells could provided over QUIC instead of TCP. He found that it's much easier and more efficient to design secure shells over HTTP/3 and QUIC instead of directly QUIC. His findings are available in a [technical report](https://arxiv.org/abs/2312.08396) and a [prototype in go](https://github.com/francoismichel/ssh3).

Flavio Luciani, who manages the Italian IXP shared some [statistics](https://www.linkedin.com/posts/flavio-luciani-19681213a_fortnite-activity-7137737374242881537-esPV?utm_source=share&utm_medium=member_desktop) to compare the traffic on his IXP caused by a new release of Fortnite and football matches in Italy. Clearly IP Multicast could allow to reduce this huge load.

![fortnite-foot.png]({{site.baseurl}}/images/fortnite-foot.png)

As in previous years, [Cloudflare](https://www.cloudflare.com) shares some of their findings during 2023 in their annual [year in review](https://blog.cloudflare.com/radar-2023-year-in-review). This year's main points include a 25% growth of the Internet traffic, half of the web requests use HTTP/2and 20% HTTP/3, 70% of the web requests from India used IPv6, ...

As discussed in a [previous post](http://blog.computer-networking.info/ipv6-optus-etc/), major web browsers have started to deploy encrypted ClientHellos. Kathleen Moriarty discusses the [implications of this deployment in enterprise networks](https://labs.ripe.net/author/kathleen_moriarty/security-control-changes-due-to-tls-encrypted-clienthello/).

Students often use [Wireshark](https://www.wireshark.org) to analyze packet traces. Wireshark is an open-source that welcomes contributions, but given the size of the complexity of this project, it can be difficult for newcomers to contribute. Uli Heilmeier provides several hints on how to contribute to Wireshark in a presentation entitled [Contribute to Wireshark - the low hanging fruits](https://weberblog.net/contributing-to-wireshark-without-any-coding-skills/) that he gave at the [Wireshark Developer and User Conference](https://sharkfest.wireshark.org/sfeu/).

Latency is a key performance factor in today's Internet. Catalina Alvarez explains in a [blog post](https://pulse.internetsociety.org/blog/how-gamers-provide-a-snapshot-of-internets-health) how they developed a system to extract latency information from publicly available images made available by Twitch. 


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 
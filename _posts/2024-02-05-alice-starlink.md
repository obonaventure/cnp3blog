---
layout: post
title: Openwrt routers, Media over QUIC, new Fiber optics record, Alice and Bob, Starlink lasers
tag: History, Linux, QUIC, cryptography, laser, satellite, optical fiber
author: Olivier Bonaventure
---

The [Openwrt project](https://openwrt.org) provides a custom Linux distribution that is targeted for routers. The xDSL or cable router that your ISP gave you probably uses a variant of Openwrt. The Openwrt distribution runs on a wide range of routers, from very small ones to enterprise routers. Recently, the leaders of the project have announced that they were working on a [reference router](https://lwn.net/ml/openwrt-devel/a8aaa495-da0b-4ddc-8c4f-3e1192d8b012@phrozen.org/) that would be commercially available. If you plan to experiment with open routers, this could become your preferred platform.

The [QUIC protocol](https://www.rfc-editor.org/rfc/rfc9000.html) was designed with web services in mind. It is widely used by large cloud providers and content distribution networks. In parallel, QUIC is also used to support DNS resolution. For example, recent Android smartphones used [DNS over HTTP/3 and thus QUIC](https://security.googleblog.com/2022/07/dns-over-http3-in-android.html) by default and Apple uses [QUIC for iCloud private relay](https://www.apple.com/privacy/docs/iCloud_Private_Relay_Overview_Dec2021.PDF). The IETF also explores how QUIC could be used to support media services within the [Media over QUIC working group](https://quic.video). A recent [IETF blog post](https://www.ietf.org/blog/moq-overview/) describes the main principles of this effort in details.

Students often wonder how much data can be carried on a standard optical fiber. Last week, [NICT](https://www.nict.go.jp) announced that they established a new record by [transporting 301 Tbps on a commercial optical fiber](https://www.nict.go.jp/en/press/2024/01/29-1.html?utm_source=hootsuite&utm_medium=twitter&utm_term=&utm_content=&utm_campaign=). The post provides additional information on the technology used to break this record.

Cryptographers and teachers often provide examples of security protocols using characters that include Alice and Bob. This habit comes from Diffie and Hellman seminal paper on [New directions in cryptography](https://ieeexplore.ieee.org/document/1055638). A [nice website](http://cryptocouple.com) discusses the history of Alice and Bob journey through various security protocols.

The first Starlink satellites were simple relays operating at the physical layers. Homes would use their satellite antenna to send a wireless signal that was amplified and directed to a ground station that was connected to the Internet. Recent Starlink satellites go one step further in creating a network around the Earth. They are equipped with laser beams that they use to create inter-satellite links. In some parts of the world, Starlink users are two or maybe three hops from the ground station that is connected to the Internet. Few technical information has leaked about the operation of these inter-satellite links. A [recent presentation in Australia](https://au.pcmag.com/networking/103636/starlinks-laser-system-is-beaming-42-million-gb-of-data-per-day) provides some information about these inter-satellite links.

[AMS-IX](https://www.ams-ix.net), one of the largest Internet eXchange Points celebrates its 30th birthday. They provide on their website a short [timeline](https://events.ams-ix.net/30-years) with some of the key moments of their history.



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
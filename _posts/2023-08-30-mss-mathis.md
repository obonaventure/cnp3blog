---
layout: post
title: A closer look at TCP Maximum Segment Sizes in the wild 
tag: TCP
author: Olivier Bonaventure
---

A [TCP](https://beta.computer-networking.info/syllabus/default/protocols/tcp.html) connection always starts with the three-way handshake. The client sends a SYN packet contains several TCP options including the Maximum Segment Size (MSS) that announces the largest segment that the client agrees to receive. The server provides its own MSS in the SYN+ACK.

The MSS is usually derived from the Maximum Transmission Unit (MTU), i.e. the largest packet that the host can send on its Local Area Network. In a [post on the tcpm mailing list](https://mailarchive.ietf.org/arch/msg/tcpm/Hjyn-bqa7Ndb5rSDToIPxHgSD1g/), Matt Mathis provided very interesting data on the MSS sizes observed on the [Measurement Lab](https://www.measurementlab.net/) servers.

With IPv4, the most frequent values are :

 - 1448 bytes
 - 1460 bytes
 - 1440 bytes

With IPv6, the most frequent values are :

 - 1428 bytes
 - 1440 bytes
 - 1288 bytes

In both cases, the measurements indicate that there are many other values of the MSS option that the measurement lab servers receive. The full data is available as a [csv file](https://mailarchive.ietf.org/arch/msg/tcpm/Hjyn-bqa7Ndb5rSDToIPxHgSD1g/4/). If you have free time, you can try to infer in which scenario a given host would send a given value of the MSS. Note however that there are middleboxes (such as CPE routers, enterprise firewalls, ...) that will happily change the MSS option in all SYN segments that they forward to prevent some types of fragmentation problems. You can use [tracebox](http://www.tracebox.org) to detect this type of interference.


You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 
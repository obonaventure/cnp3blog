---
layout: post
title: TFO deployment seems to grow
tag: tcp
author: Olivier Bonaventure
---

[TCP](https://www.computer-networking.info/2nd/html/protocols/tcp.html) Fast Open is a TCP extension that enables client and servers to place data inside the SYN and SYN+ACK packets during the three-way handshake. This extension has been pushed by google to speedup short transfers. It is defined in [RFC7413]() and its Linux implementation is described in a [nice LWN.net article](https://old.lwn.net/Articles/508865/).

Despite being supported by Linux and Apple, it has not yet been widely deployed on servers. [Recent measurements](https://twitter.com/netray_io/status/1160954722074943488?s=09) collected by [netray.io](https://netray.io) show that a growing number of servers support TFO.


![TFO is rising]({{ site.baseurl }}/images/tforising.png)
- source: [https://twitter.com/netray_io/status/1160954722074943488?s=09](https://twitter.com/netray_io/status/1160954722074943488?s=09)



*This blog post was written to inform the readers of [Computer Networking : Principles, Protocols and Practice](https://www.computer-networking.info) about the evolution of the field. You can subscribe to the Atom feed for this blog at [https://obonaventure.github.io/cnp3blog/feed.xml](https://obonaventure.github.io/cnp3blog/feed.xml). These notes are also posted on [the ebook's Facebook page](https://www.facebook.com/Computer-Networking-Principles-Protocols-and-Practice-129951043755620/)*
---
layout: post
title: Text compression using large language models, Starlink as a cellular access network, the FreeBSD networking stack, 
tag: compression, satellite, freebsd
author: Olivier Bonaventure
---

Data compression algorithms are widely used by Internet applications to reduce the amount of data that needs to be exchanged over the network. The [Gzip](https://www.rfc-editor.org/rfc/rfc1952) format is one of these popular techniques. New compression schemes have been proposed during the last years. [Fabrice Bellard](https://bellard.org) explores a new approach that improves the compression ratio for text files. The [ts_zip](https://bellard.org/ts_zip/) software uses a large language model to efficiently compress files. It achieves excellent compression ratios, but uses more memory and almost requires a GPU.


Low-orbit satellites are already commercially used to provide Internet access and emergency cell phone services. The next step could be to provide Cellular access using low orbit satellites. Elon Musk shared early results on a tweet.

![Cellular in starlink]({{site.baseurl}}/images/Starlink-cellular.png)

This could be interesting in regions that are far away from the networking infrastructure. However, if you believe that your cellular towers will be replaced by low-orbit satellites, please bear in mind that several hundreds of the Starlink satellites [have already been decommissioned](https://www.theverge.com/2024/2/13/24072033/spacex-will-dispose-of-100-orbiting-starlink-satellites-because-of-an-undisclosed-glitch). This means that these satellites have burned in our atmosphere, which creates [another form of pollution in our atmosphere](https://www.pnas.org/doi/full/10.1073/pnas.2313374120).

For many years, the standard TCP/IP stack was the one included in the Unix BSD distribution. During the 1980s, 1990s and early 2000s, this stack was the most stable TCP/IP stack. Since then, other stacks have evolved. The Linux TCP/IP stack is used by a wide range of servers and also by Android smartphones and OpenWRT routers. Apple uses its own stack which was derived from the Unix BSD stack and Microsoft uses its own stack. The BSD stack continues to evolve with the [FreeBSD](https://www.freebsd.org), [OpenBSD](https://www.openbsd.org) and [NetBSD](https://www.netbsd.org) which are the children of the Unix BSD kernel. The latest issue of the [FreeBSD journal](https://freebsdfoundation.org/our-work/journal/) contains [several articles on the FreeBSD TCP/IP stack](https://freebsdfoundation.org/past-issues/networking-10th-anniversary/). An interesting read for students willing to explore different TCP/IP stacks.

For more than six decades, the [Association for Computing Machinery](https://www.acm.org) has published its [Communications of the ACM](https://cacm.acm.org) every month. This magazine was initially published only on paper and was available to subscribers and in libraries. The ACM has finally decided to [distribute CACM as an open access journal](https://cacm.acm.org/news/cacm-is-now-open-access-2/). CACM is probably the best way to stay up to date with the evolution of Computer Science. It regularly publishes networking articles and covers the entire Computer Science field.


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
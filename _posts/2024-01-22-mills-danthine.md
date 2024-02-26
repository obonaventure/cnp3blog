---
layout: post
title: Detecting black holes, IP addressing space, Dave Mills and André Danthine
tag: black holes, IPv4, IPv6, NTP
author: Olivier Bonaventure
---

Black holes and packet losses are annoying problems in production networks. They can be caused by hardware problems, configuration problems and are difficult to debug. To help network operators troubleshoot such problems, Juniper routers provide a [resiliency interface](https://community.juniper.net/blogs/julian-lucek/2024/01/02/detection-of-blackholes-in-networks-using-jri) that exports information about the packets that were discarded by a router and why they were discarded.

Geoff Huston published his [annual report on the evolution of the IP addressing space](https://blog.apnic.net/2024/01/17/ip-addresses-through-2023/). The transfers of IPv4 address blocks have declined in 2023 compared to 2022 and the cost of IPv4 addresses also has started to decline.  

![Price of IPv4 addresses]({{site.baseurl}}/images/ipv4-price.png)

The dataset also provides interesting information such as the number of IPv4 addresses per human. While the USA uses 43.8% of the IPv4 addressing space, this is only 4.73 addresses per inhabitant. The Vatican has 20.6 addresses per inhabitant and Seychelles 67.46...

![IPv4 addresses per inhabitant]({{site.baseurl}}/images/ip-perhuman.png)

Looking at IPv6, it is interesting to observe that The Netherlands seems to lead
with 43.9 /48 IPv6 blocks per inhabitant.

![IPv6 addresses per inhabitant]({{site.baseurl}}/images/ip6-country.png)

A last and positive point from this report is that IPv6 deployment continues
to grow with 36% of Internet users who prefer to use IPv6. In China, the city
of [Xiong'an, which is supposed to be a model for future digital cities, uses
IPv6 only](https://blog.apnic.net/2024/01/23/the-ipv6-city-xiongan-china/).

![IPv6 deployment]({{site.baseurl}}/images/ip6-deployment23.png)

Unfortunately, this post ends with two sad news items. Dave Mills, who led the design,
implementation and deployment of time synchronization protocols [passed away last
week](https://elists.isoc.org/pipermail/internet-history/2024-January/009265.html). Arstechnica published a [short article on his achievements](https://arstechnica.com/gadgets/2024/01/inventor-of-ntp-protocol-that-keeps-time-on-billions-of-devices-dies-at-age-85/). The New Yorker published an interesting article entitled [The thorny problem of keeping the Internet's Time](https://www.newyorker.com/tech/annals-of-technology/the-thorny-problem-of-keeping-the-internets-time) in 2022 about his efforts. [André Danthine](https://www.run.montefiore.uliege.be/People/PersonalPages/index.php?tri=ADA), one of the pioneers of networking research in Europe [also passed away](https://elists.isoc.org/pipermail/internet-history/2024-January/009297.html). In 2012, he gave a [long interview to Andrew Russell](https://conservancy.umn.edu/bitstream/handle/11299/162412/oh428ad.pdf?sequence=3&isAllowed=y) discussing his contributions. 


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.

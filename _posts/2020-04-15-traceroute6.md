---
layout: post
title: Another interesting traceroute
tag: ipv6, traceroute
author: Olivier Bonaventure
---

Network engineers sometimes have fun in configuring networks. In 2013,
Ryan Weber configured a set of Cisco VRFs to return a Star Wars story
when running ```traceroute``` towards a specific IP address.
You can find some technical information in [an article published by
The Register](https://www.theregister.co.uk/2013/02/15/star_wars_traceroute/)
and a [youtube video](https://www.youtube.com/watch?v=_QevOSlLTFw).

Recently, [Ambrose Chua](https://twitter.com/serverwentdown) posted a
similar traceroute, this time using IPv6.

````
traceroute6 who.makerforce.io
traceroute6 to who.makerforce.io (2001:470:ed5d:101::2c) from 2a02:2788:484:b4f:d953:ebbb:7040:42a7, 64 hops max, 12 byte packets
 1  *
    host.dynamic.voo.be  75.408 ms  4.746 ms
 2  host.dynamic.voo.be  24.797 ms  8.862 ms  7.920 ms
 3  host.dynamic.voo.be  10.954 ms  9.338 ms  8.602 ms
 4  2a02:2788:ffff:18::1  32.104 ms  11.161 ms  5.932 ms
 5  e0-54.core1.ams2.he.net  15.120 ms *  16.485 ms
 6  * * *
 7  100ge9-2.core1.par2.he.net  18.813 ms  19.388 ms  34.158 ms
 8  100ge2-2.core1.mrs1.he.net  30.247 ms  34.988 ms  36.274 ms
 9  100ge14-2.core1.sin1.he.net  171.610 ms  166.838 ms  163.820 ms
10  tserv1.sin1.he.net  166.171 ms  163.923 ms  166.422 ms
11  tunnel409638-pt.tunnel.tserv25.sin1.ipv6.he.net  184.450 ms  186.132 ms  170.982 ms
12  hey.there.my.name.is.ambrose  197.241 ms  174.491 ms  171.213 ms
13  and.i.really.like.computer.networks  167.645 ms  172.592 ms  179.381 ms
14  seems.like.you.like.them.too  172.236 ms  175.729 ms  176.240 ms
15  how.else.would.you.be.here  187.832 ms  201.054 ms  205.484 ms
16  i.knew.it  175.159 ms  173.445 ms  174.957 ms
17  i.knew.it.all.along  178.683 ms  172.275 ms  177.511 ms
18  maybe.ill.start.with.some.lyrics  167.188 ms  170.388 ms  168.392 ms
19  a.long.long.time.ago  175.124 ms  173.319 ms  170.774 ms
20  i.could.still.remember  166.268 ms  176.394 ms  168.319 ms
21  when.my.laptop.could.connect.elsewhere  170.340 ms  175.583 ms  176.445 ms
22  and.i.tell.you.all.there.was.a.day  203.471 ms  175.647 ms  179.737 ms
23  the.network.card.i.threw.away  175.205 ms  192.105 ms  169.631 ms
24  had.a.purpose.and.it.worked.for.you.and.me  180.352 ms  189.776 ms  171.628 ms
25  but.29.years.completely.wasted  172.676 ms  174.497 ms  167.763 ms
26  with.each.address.weve.aggregated  180.999 ms  191.620 ms  202.941 ms
27  the.tables.overflowing  170.898 ms  179.537 ms  178.583 ms
28  the.traffic.just.stopped.flowing  177.370 ms  173.273 ms  175.784 ms
29  and.now.were.bearing.all.the.scars  200.170 ms  186.248 ms  183.810 ms
30  and.all.my.traceroutes.showing.stars  180.080 ms  186.954 ms  177.057 ms
31  * * *
32  the.packets.would.travel.faster.in.cars  168.251 ms  174.419 ms  178.223 ms
33  the.day.the.routers.died  176.157 ms  179.607 ms  195.054 ms
```

This traceroute output contains the song [The day the router died](https://www.youtube.com/watch?v=_y36fG2Oba0) that
was played by the secret working group at RIPE 55 in 2007 to encourage
network engineers to deploy IPv6. Clearly an important song in the networking
folklore...


Ambrose Chua also released the [script](https://gist.github.com/serverwentdown/3febf9988ea0230aac790a1b4f3e1e22)
used to produce the required network configuration. Given that each home user
receives a /64 or a /56 we have lots of IPv6 addresses to play with...





*This blog post was written to inform the readers of [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) about the evolution of the field. You can subscribe to the [Atom feed for this blog](http://blog.computer-networking.info/feed.xml).
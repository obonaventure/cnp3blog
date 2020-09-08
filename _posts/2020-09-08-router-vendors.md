---
layout: post
title: ISP networks use diverse routers 
tag: traceroute
author: Olivier Bonaventure
---

Routers are an important part of the Internet infrastructure as they
carry all the packets that we exchange. Several vendors sell these routers.
Some vendors supply different types of routers while others focus on
specialised ones such as access routers or backbone routers. Industry analysts
often publish market studies that provide the market share of each vendor.

In parallel with these market analysis, Internet measurements also reveal how
routers are deployed. Two recent papers studied the diversity of the routers
in ISP networks.

In [Network Fingerprinting: Routers under Attack](https://orbi.uliege.be/handle/2268/248733), Emeline Marechal and Benoit Donnet analyze the ICMP messages that routers return. Earlier work has shown that different types of routers use different TTL values when generating such messages.

 - Cisco routers use a TTL of 255 when sending ICMP time exceeded or echo-reply messages
 - Juniper routers running JUNOS use a TTL of 255 when sending ICMP time exceeded and a TTL of 64 when sending ICMP echo-reply messages
 - Brocade, Alcatel and Linux use a TTL of 64 when sending ICMP time exceeded or echo-reply messages

Using these signatures, they infer the distribution of routers in large ISPs based on traceroute measurements. One of the important results of the paper is that ISPs use routers from different vendors as shown in the below figure.

![Routers per AS]({{ site.baseurl}}/images/router-as.png)

Another related work is [Classifying Network Vendors at Internet Scale](https://arxiv.org/abs/2006.13086) by Jordan Holland and his colleagues. This paper takes a different approach as it collects ssh, telnet and SNMP banners from routers to infer their vendors. Their dataset also confirms the diversity of ISP routers.

![Router vendors]({{ site.baseurl}}/images/router-table.png)


*This blog post was written to inform the readers of [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) about the evolution of the field. You can subscribe to the [Atom feed for this blog](http://blog.computer-networking.info/feed.xml).
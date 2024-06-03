---
layout: post
title: Class E IPv4 addresses, DNSSEC, RPKI covers 50% of BGP prefixes, looking back at the last 50 years of TCP, consensus in Internet standards, 600,000 access routers destroyed in 3 days
tag: ip, dnssec, rpki, bgp, tcp, consensus, quic
author: Olivier Bonaventure
---

The exhaustion of the IPv4 addressing space remains a problem in various parts of the Internet. With prices reaching about 10,000$ for a /24 IPv4 subnet, network engineers explore various alternations. One of them is to reuse blocks of IPv4 addresses that are not actively used. This includes the 240.0.0.0/4 (aka Class E) block of IPv4 addresses. This block was reserved for future use by [RFC1112](https://www.rfc-editor.org/rfc/rfc1112.html) and still remains reserved in the [current table of the IPv4 addresses allocations](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xhtml). In a [blog post](https://blog.benjojo.co.uk/post/class-e-addresses-in-the-real-world) and a RIPE presentation, Ben Cartwright-Cox discusses the problem of reusing the 240.0.0.0/4 address block in details. While there will be hacks to extend the IPv4 addressing space, the deployment of IPv6 everywhere remains the best solution to the address exhaustion problem.

In 1994, the IETF started to work on developing extensions to secure the DNS. In 1997, the Domain Name Security Extensions were published as [RFC2065](https://datatracker.ietf.org/doc/html/rfc2065). After that, DNSSEC evolved slowly and today the most popular DNS names are still not signed using DNSSEC. In a very interesting [blog post](https://blog.apnic.net/2024/05/28/calling-time-on-dnssec/), Geoff Huston looks back at the evolution of DNSSEC and explores some of the reasons why DNSSEC did not became widely used and even wonders whether we should stop trying to deploy DNSSEC...


The Resource Public Key Infrastructure (RPKI) is an key element in securing the BGP routing infrastructure. Thanks to the RPKI, network operators can verify that the BGP announcements that they receive from certain prefixes were issued by the Autonomous System that is officially responsible for this prefix. In early May 2024, the RPKI reaches an important milestone with more than 50% of the BGP prefixes covered by the RPKI. You can track the RPKI coverage on [https://rpki-monitor.antd.nist.gov/](https://rpki-monitor.antd.nist.gov/). A detailed discussion of this milestone appears on the [kentik blog](https://www.kentik.com/blog/rpki-rov-deployment-reaches-major-milestone/)

![RPKI]({{site.baseurl}}/images/rpki50.png)

In a recent [blog post](https://www.kentik.com/blog/times-up-how-rpki-roas-perpetually-are-about-to-expire/), Doug Madory and Job Snijders use publicly available data to explore the expiration times of Route Origin Attestations provided by different Regional Internet Registries.

Mark Nottingham, Lucas Pardue and Marwan Fayez discuss the evolution of TCP during the last 50 years and look at the impact of QUIC in a one hour [video discussion](https://www.youtube.com/watch?v=qlsfeDYAMFE).

Internet standards, published mainly by the [Internet Engineering Task Force](https://www.ietf.org), are developed by hundreds of engineers who discuss regularly by email and meet three times per year. A key component of the Internet standardization process is the consensus. Mark Nottinghan explains how this consensus is achieved in [an interesting blog post](https://www.mnot.net/blog/2024/05/24/consensus).


Dan Goodin reports on [arstechnica.com](https://arstechnica.com) that in October last year some malware managed to destroy 600,000 access routers in the Windstream ISP in the US. The [article](https://arstechnica.com/security/2024/05/mystery-malware-destroys-600000-routers-from-a-single-isp-during-72-hour-span/) provides some additional information about the attack based on a recently published report. This is a very unusual attack that targeted a single ISP and forced the replacement of 600,000 access routers. 


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
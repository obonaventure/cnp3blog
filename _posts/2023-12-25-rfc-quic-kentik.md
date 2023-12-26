---
layout: post
title: BGP, RPKI, Web PKI, RFCs, DHCP, SMTP and QUIC
tag: PKI, BGP, failures, SMTP, QUIC
author: Olivier Bonaventure
---


This end of the year brings several interesting reports that look back at 2023. [Kentik](https://www.kentik.com) provides its [annual report](https://www.kentik.com/blog/a-year-in-internet-analysis-2023/) that analyzes important outages that affected the Internet, BGP and the RPKI. [MeasurementLab](https://www.measurementlab.net) also summarizes the [new types of tests that they added to the platform in 2023 and their plans for 2024](https://www.measurementlab.net/blog/end-of-year-letter-2023/).

Russ White describes in [a series of blog posts](https://www.measurementlab.net/blog/end-of-year-letter-2023/) the process of extending Internet standards from an idea to its publication as an RFC. A interesting read for those willing to better understand how Internet standards are developed. Most published RFCs focus on Internet protocols, but some of them provide reflections and viewpoints on the evolution of the Internet architecture. [RFC9518](https://www.rfc-editor.org/rfc/rfc9518.html) analyzes the tension between centralization and decentralization on the Internet. While the Internet was created as a decentralized network, there are several forces that could encourage the centralization of some parts of the Internet around a few actors. The document then discusses how the role that Internet standardization can play to preserve a decentralized Internet.

The Web PKI and the Resources PKI play an important role in securing our TLS serves and interdomain routing. Two recent documents discuss these important infrastructures. Geoff Huston analyzes the trust models of the RPKI in a [blog post](https://labs.ripe.net/author/gih/models-of-trust-for-the-rpki/). Princeton's Center for Information Technology Policy summarizes in a long [report](https://citpsite.s3.amazonaws.com/Securing+Public+Key+Infrastructure+Report.pdf) the main results of a recent workshop on Securing the Public Key Infrastructure.

The DHCP protocol allocates IP addresses to hosts in a network. However, there are sometimes situations where DHCP does not work as expected. Robert Graham shared an [analysis of a DHCP configuration problem](https://x.com/erratarob/status/1739132876732674539?s=61&t=tP_GnIXAFXsfnvztGctEig) that Internet to be usable on a plane. This problem could affect many other DHCP deployments.

![DHCP issue]({{site.baseurl}}/images/x-dhcp.png)

Large DNS services are now delivered using anycast. This means that a set of DNS servers or resolvers use the same unicast address and routers are configured to deliver requests to the closest DNS server. However, twenty years ago most DNS services were provided by isolated servers. Lars-Johan Liman shares on the [RIPE glob](https://labs.ripe.net/author/liman/anycast-dns-for-the-iroot-service-20-years-of-100-availability/) his experience in providing the iRoot DNS service using anycast during the last 20 years.

Security problems often force to read in more details protocol specifications and reconsider some implementation choices. The recently announced [SMTP smuggling attack](https://sec-consult.com/blog/detail/smtp-smuggling-spoofing-e-mails-worldwide/) affects various SMTP servers. By exploiting how SMTP messages are handled, attackers can send spoofed messages through vulnerable servers. The sec-consult [blog post](https://sec-consult.com/blog/detail/smtp-smuggling-spoofing-e-mails-worldwide/) describes the attack in details. Another recent problem affects the [QUIC protocol](https://www.rfc-editor.org/rfc/rfc9000.html). QUIC supports a connection migration features that allows a smartphone to easily switch from Wi-Fi to cellular when interacting with a server. This handover capability relies on the [connection migration](https://www.rfc-editor.org/rfc/rfc9000.html#name-connection-migration) mechanism. Martin Seemann has recently described [a vulnerability](https://seemann.io/posts/2023-12-18-exploiting-quics-path-validation/) that affects implementations of this mechanism and could cause denial of service attacks.



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 
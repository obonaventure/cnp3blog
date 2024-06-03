---
layout: post
title: Modern SMTP, Gigabit Ethernet, Google stops using route servers at IXPs, Green software, Cryptography with openssl, Ballad of UDP and TCP
tag: smtp, ethernet, humor
author: Olivier Bonaventure
---


The [SMTP protocol](https://beta.computer-networking.info/syllabus/default/protocols/email.html?highlight=smtp) is one of the oldest Internet protocols. It is still used to deliver email messages. However, due to the proliferation of spam, modern SMTP servers need to complement SMTP with various different protocols to perform different types of checks on the messages that are sent. Large email providers like GMail or Yahoo use they additional protocols, which puts a barrier to companies willing to deploy and manage their own SMTP server. A [recent blog post](https://www.xomedia.io/blog/a-deep-dive-into-email-deliverability/) summarizes the requirements imposed by large email providers.

Gigabit Ethernet over twisted pair, aka 1000Base-T, is becoming the basic solution to provide wired connectivity. To carry data at 1000 Mbps, the engineers who developed Gigabit Ethernet have included various optimizations in the physical layer. A series of [three blog posts](https://lostintransit.se/2024/03/28/1000base-t-part-1/) provides a nice overview of Gigabit Ethernet, including a [detailed description of the physical layer](https://lostintransit.se/2024/04/02/1000base-t-part-2/) and an explanation of the [auto-negotiation techniques](https://lostintransit.se/2024/04/03/1000base-t-part-3/).

Internet eXchange Points often rely on route servers to allow the participating ISPs to efficiently exchange routers without creating many eBGP sessions. Many were surprised when Google announced that they started to stop using the route servers and preferred to use direct eBGP sessions. More information in a [recent blog post](https://anuragbhatia.com/post/2024/04/google-to-stop-peering-via-rs/).

[IEEE Spectrum](https://spectrum.ieee.org/) published an [interesting article](https://spectrum.ieee.org/green-software) that discusses directions to make software, but various web and cloud services greener by using less energy and emitting few carbon dioxide. The article points several tools to estimate the carbon impact of services. For example, [ecograder](https://ecograder.com) estimates the carbon impact of web sites. The [Networking Notes blog] that you read [costs 0.24 grams of carbon dioxide per page load](https://ecograder.com/report/rCS8i2x20HmmzcZexOqvoslb).

![Ecograder results]({{site.baseurl}}/images/ecograder.png)

Cryptography is used in many network protocols. Network engineers need to understand the basics of cryptography. A nice way to understand how cryptography works in the real world is to be able to experiment with real implementations. A series of [three blog posts](https://sergioprado.blog/introduction-to-encryption-for-embedded-linux-developers/) describes the basics of cryptography and how to use it with openssl. The [blog post](https://sergioprado.blog/a-hands-on-approach-to-symmetric-key-encryption/) focuses on symmetric encryption techniques while the [third post](https://sergioprado.blog/asymmetric-key-encryption-and-digital-signatures-in-practice/) explains asymmetric encryption and digital signatures.


To conclude this post, we encourage you to listen to the [Ballad of UDP and TCP](https://app.suno.ai/song/18ff7842-6efc-4886-97b5-944e1a6fc18d/), a short song about networking protocols.


![Ballad of UDP and TCP]({{site.baseurl}}/images/ballad.png)


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
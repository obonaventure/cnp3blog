---
layout: post
title: Be aware of RPKI, DNS and PNG
tag: BGP, RPKI, password, dns, java
author: Olivier Bonaventure
---

During the first week of 2024, Orange Spain suffered from a sudden traffic drop. This is illustrated by the figure below from cloudflare's radar.

![Traffic drop in Orange Spain]({{site.baseurl}}/images/cloudflare-orange.png)


This sudden drop of traffic was caused by an unusual problem that affected Orange Spain. During the last few years, many operators have deployed the RPKI to secure interdomain routing, which improves the security of interdomain routing. Network operators use the RPKI to bind their AS number to the IP prefixes that they advertise. A growing number of network operators use the RPKI to validate the BGP messages that they receive and a fraction of them block the BGP announcements that originate from AS numbers that do not match the RPKI data. This has prevented hijacking attacks.

Network operators can publish the RPKI data for their prefixes directly or use third parties such as Internet Routing Registries like RIPE. Orange Spain did not publish RPKI information for its prefixes but suffered from a new type of attack. Orange Spain used a weak password on the RIPE website and an attacker impersonated Orange Spain on RIPE servers to publish a fake Route Origin Authorization that associated some IP prefixes from Orange Spain to a different AS number. As Orange Spain used their regular AS number to advertise this prefix, BGP routers from ASes that use RPKI considered this announcement to be invalid according to the RPKI information and rejected the BGP announcement. The weak password used by Orange Spain resulted in a new form of denial of service attack...

Several network engineers published an interesting analysis of this new type of attack:

 - Doug Madory's analysis on the [kentik blog](https://www.kentik.com/blog/digging-into-the-orange-espana-hack/)
 - Dan Goodin's [article on ars technica](https://arstechnica.com/security/2024/01/a-ridiculously-weak-password-causes-disaster-for-spains-no-2-mobile-carrier/)
 - [Another analysis](https://www.bortzmeyer.org/orange-espagne-bgp.html) by Stéphane Bortzmeyer (in French)


In addition, Job Snijder's [summary](https://mailman.nanog.org/pipermail/nanog/2024-January/224318.html) of the evolution of RPKI in 2023 is also very interesting.


If you own IP prefixes, make sure that you use strong passwords and two-factor authentication on the Internet Routing Registries...

The [Domain Name System](https://beta.computer-networking.info/syllabus/default/protocols/dns.html) is one of the key Internet protocols. This is also an example of a protocol that sends binary messages inside UDP segments. The format of the DNS messages has been optimized to reduce the length of the UDP segments. The DNS is also a nice example for students willing to write a first implementation that sends real Internet protocol messages. A [recent blog](https://beta.computer-networking.info/syllabus/default/protocols/dns.html) describes how to encode DNS messages in Java.


The web contains many images using the PNG format. Evan Hahn provides a detailed description of this file format by looking at the contents of the [smallest PNG file](https://evanhahn.com/worlds-smallest-png/) that has a length of 67 bytes.



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.

---
layout: post
title: Design flaw in DNSSEC implementations, access network standards
tag: DNSSEC, security, access networks
author: Olivier Bonaventure
---


The [Domain Name System](https://beta.computer-networking.info/syllabus/default/protocols/dns.html) is one of the key protocols in today's Internet. DNS allows to map names onto addresses. Twenty-five years ago, the [IETF](https://www.ietf.org) adopted [DNSSEC](https://beta.computer-networking.info/syllabus/default/protocols/dnssec.html) to secure the DNS infrastructure. DNSSEC allows resolvers and DNS clients to authenticate DNS responses using public-key cryptography. Measurements indicate that about 31% of domains use DNSSEC. To cope with changes in the public keys used by DNS servers, DNSSEC allows a server to advertise different keys that it uses to sign its responses. When a resolver or a client receives a response, it must try to validate the response using any of the announced public keys. Unfortunately, [German researchers](https://labs.ripe.net/author/haya-shulman/keytrap-algorithmic-complexity-attacks-exploit-fundamental-design-flaw-in-dnssec/) have recently announced that this can be abused by attackers to consume a huge amount of CPU on DNS resolvers that validate DNSSEC responses. With specially crafted DNSSEC messages, they even managed to consume up to 16 CPU hours using a single DNS response. This is probably the worst problem that affects DNSSEC. Since the root of the problem is part of the DNSSEC specification, all DNSSEC implementations that validate DNSSEC messages are vulnerable to this attack. The attack requires a specially crafted DNS zone. The IETF will need to update the DNSSEC specifications and see whether this type of attack is possible on other related protocols.

Randy Bush replied to this post indicating that the problem was anticipated by the DNS designers. Indeed, [RFC1034](https://datatracker.ietf.org/doc/html/rfc1034) contains the following note :

 
    RFC 1034 §5.3.3 Step 2 .1

    Bound the amount of work (packets sent, parallel processes started)
    so that a request can't get into an infinite loop or start off a
    chain reaction of requests or queries with other implementations
    EVEN IF SOMEONE HAS INCORRECTLY CONFIGURED SOME DATA.

This important paragraph has been forgotten by DNSSEC implementors. Some implementations have been updated and are listed in the [CVE-2023-50387 page](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-50387).


If you'd like to deploy your own DNS and DHCP servers, [arstechnica.com](https://arstechnica.com) published an interesting step-by-step [article](https://arstechnica.com/information-technology/2024/02/doing-dns-and-dhcp-for-your-lan-the-old-way-the-way-that-works/) on how to deploy these key services in a home network.


The [Broadband Internet Technical Advisory Group](https://bitag.org) has published a very interesting [report](https://bitag.org/broadband_technologies.php) that summarizes the main broadband access technologies that are and have been deployed, using cable, DSL, fiber, satellites, wireless, ... The report contains several very useful  tables with the key characteristics of the main access technologies. 



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
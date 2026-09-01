---
layout: post
title: Networking notes for September 2026
tag: privacy, community, resilience, socket, bgp, spanning tree, dns
author: Olivier Bonaventure
---


Speedtest is a well known app to measure throughput. A [recent study](https://lnkd.in/p/eHKeSW5z) shows that it also collects information from the temperature sensor on Android smartphones. What are the other information that Speedtest collects once it has been installed on your smartphone?


An interesting article in IEEE Spectrum explains how a community in Fort Albany built [its own fiber optic network](https://spectrum.ieee.org/indigenous-fiber-network) to provide high speed Internet access to the entire city in a remote area North of Canada.

In August, a solar eclipse was visible in Europe with peaks in Iceland, Spain and Portugal. Cloudflare's [blog](https://blog.cloudflare.com/total-eclipse-internet-traffic-iceland-spain-portugal/) shows that Internet usage reduced during the eclipse in many countries in Europe. 

As part of their annual Urban Resilience Exercises, [different regions of Taiwan suffered from major slow downs](https://focustaiwan.tw/society/202607210012) in order to evaluate the resilience of the Taiwanese society in tough times between August 7th and August 13th. This shows the growing importance of the Internet in our society. 

The IETF has published [RFC10001](https://datatracker.ietf.org/doc/rfc10001/) which provides interesting guidelines on authoritative DNS servers.

On the [APNIC blog](https://blog.apnic.net/), George Michaelson explains the [origin of the Unix sockets](https://blog.apnic.net/2026/07/28/hooray-for-the-sockets-interface/).

Internet Service Providers use BGP policies to control the flow of packets on their interdomain links. The most common policies are shared-cost and customer-provider. In 2022, the IETF published in [RFC9234](https://datatracker.ietf.org/doc/rfc9234/) a set of BGP extensions that allow to automate the configuration of these policies to prevent misconfiguration errors. A recent [blog post](https://blog.cloudflare.com/rfc9234-bgp-role-model/) analyses the deployment of this BGP extension.

Tom Mattke provides interesting guidelines on how network engineers can [tune their prompts](https://routerjockey.com/prompt-engineering-for-network-engineers/) to obtain better answers from an LLM. 


[Vincent Bernat](https://vincent.bernat.ch/) wrote a nice [tutorial on the Spanning Tree protocol](https://vincent.bernat.ch/en/blog/2026-spanning-tree) which uses a nice web-based simulator that uses the Linux implementation of this protocol to exchange real packets. A nice starting point for a lab with this important protocol.

Artem Berezin uses the information stored in the DNS, e.g. MX records, to identify which servers provide email services for the top 1M domain names. His [results](https://pulse.internetsociety.org/en/blog/2026/08/two-providers-a-stubborn-plateau-and-a-very-long-tail-email-in-the-tranco-top-1m/) show that only 22.4% of the domains manage their own mail servers. Ten years ago, 44.6% of these domains managed their own mail servers. Together, Google and Microsoft receive 38% of the emails sent to these top domain names. Mail is unfortunately getting more and more centralised… 

Fifty years ago, the [first packets were exchanged over the ARPANET using packet radio](https://stefanbohacek.online/@stefan/117167402865398907).


A [BGP hijack](https://www.virtualizor.com/blog/security-incident-bgp-hijacking/) affected [Virtualizor](https://www.virtualizor.com/blog/security-incident-bgp-hijacking/). An attacker announced a prefix owned by virtualizor that contained the 	address of its software update server. During the hijack, the attacker exposed a valid TLS certificate and clients updated the software without any cryptographic validation.An attacker announced a prefix owned by virtualizor that contained the address of its software update server. During the hijack, the attacker exposed a valid TLS certificate and clients updated the software without any cryptographic validation.


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.

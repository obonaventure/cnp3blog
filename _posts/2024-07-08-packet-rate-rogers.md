---
layout: post
title: Packet rate attacks, reflections on DNSSEC, routing incident for 1.1.1.1, lessons learned from Roger's outage in 2022
tag: ddos, dnssec, outage
author: Olivier Bonaventure
---


A recent [blog post](https://blog.ovhcloud.com/the-rise-of-packet-rate-attacks-when-core-routers-turn-evil/) from OVHCloud provides interesting information about recent DDoS attacks that targeted the OVH network. Until recently, attacks reaching 1 Tbps were rare. This is not the case anymore. During the first half of 2024, OVH saw a huge increase in the volume of attacks, with attacks of 1 Tbps and more happening almost once per day on average. This requires plenty of available network and filtering capacity to cope with such attacks. 

OVH then looked in more details about the origin of these large attacks. In the past, attacks such as the MIRAI botnet came from a large number of compromised hosts or devices. The recent attacks originated from a smaller number of sources and reached 500+ millions of packets per second. Interestingly, these attacks were composed of small packets. They were thus more difficult to filter than attacks with larger packets. Looking in more details about the origin of the recent attacks, OVH suspects that these were produced by compromised mikrotik core routers. Each of these core routers that process 10 of Gbps of traffic and can also generate 4 or 12 Mpps. This is a new source of attack traffic that requires attention from network operators. 

DNSSEC was designed in the 1990s and has been slowly deployed. In a [very interesting blog post](https://blog.apnic.net/2024/07/05/where-did-dnssec-go-wrong/), Edward Lewis analyzes this deployment and lists some of the assumptions of the protocol designers that were not verified. DNSSEC assumed that hosts could not be trusted to store private keys to sign each DNSSEC response in real-time. DNSSEC precomputes the signatures for answers, which works well for positive answers, but caused problems for negative ones. Another assumption was that DNSSEC deployment would start from clients and reach top level domains later. Today, a large fraction of the TLDs use DNSSEC.

1.1.1.1 is Cloudflare's open DNS resolver. On June 27, 2024 it became unreachable on about 300 networks in 70 countries. A [blog post](https://blog.cloudflare.com/cloudflare-1111-incident-on-june-27-2024) analyzes this incident in details and traces its origin to a route leak followed by a remotely triggered black hole. AS267613 advertised 1.1.1.1/32 to its providers and peers. This advertisement should have been blocked for several reasons by BGP routers. First, most routers filter BGP routes for prefixes longer than /24. Second, 50% of the ASes use RPKI to validate BGP updates and 1.1.1.0/24 is covered by the RPKI. Unfortunately, this was not sufficient. A second problem occurred when a tier 1 provider considered the advertisement from AS267613 as a request to black hole, i.e. block, all packets destined to 1.1.1.1/32. The tier should have never accepted such a request from an AS that does not own 1.1.1.1.   

On July 8, 2022, Rogers Communications, a major network operator serving 12 million customers had a major outage that stopped the entire fixed and wireless network. An [official report](https://crtc.gc.ca/eng/publications/reports/xona2024.htm) analyzes the technical reasons for this outage and provides recommendations that are valid for many other network operators. One of the key lessons is that it is crucial to have an out-of-band management network that network engineers can use to recover from major outages. Although it can be convenient for a network operator to use its own cellular network as a backup management network, this only works if the cellular network is completely separated from the fixed network. For Rogers, the report recommends to use an out-of-band network that is managed by another company. During the outage, they had to send SIM cards from competitors to remote sites to access them...




This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
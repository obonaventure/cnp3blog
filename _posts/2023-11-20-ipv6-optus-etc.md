---
layout: post
title: Resilience, iperf, MIRAI, Encrypted ClientHello and new DNS records  
tag: resilience, measurements, ddos, TLS, https, dns 
author: Olivier Bonaventure
---

Resilience is an important factor when evaluating Internet Service Providers. Unfortunately, it is not always easy to quantify the resilience of a given ISP by using measurements or information about the ISP. The resilience of an ISP depends on a wide range of factors and a small detail can sometimes significantly lower the resilience of an entire ISP. Often, these details are only exposed by catastrophic or unexpected events. This happened a few weeks ago when Optus, a major ISP in Australia, went offline for almost half a day. Several posts provide an analysis and attempt to explain the reasons for this outage: [a detailed blog by kentik](https://www.kentik.com/blog/digging-into-the-optus-outage/) show the impact on traffic and BGP and a short [article on LightReading](https://www.lightreading.com/mobile-core/singtel-s-own-internet-exchange-crashed-optus-reports) points a possible culprit.

Researchers and network engineers often turn to iperf to measure the performance of Internet protocols on various types of networking technologies. A recent post by Simon Leinen on LinkedIn reminded us that there are two very different versions of iperf.

 - [iperf3](https://iperf.fr) supports TCP, SCTP and UDP. It can measure bandwidth, packet looses and delay jitter with UDP. It supports multicast with UDP.
 - [iperf2](https://iperf2.sourceforge.io/IperfCompare.html) supports TCP and UDP, but not multicast. It can perform tests with isochronous traffic, TCP bounce back and other tests. It also provides more metrics, including latency than iperf3. See the [detailed comparison between the two iperfs](https://iperf2.sourceforge.io/IperfCompare.html)

The next time you plan to use iperf, take a few seconds to select the tool that best matches your requirements instead of using the first result from your favorite search engine.

The MIRAI botnet is often used as a case study when analyzing Distributed Denial of Service attacks. A [Wired article](https://www.wired.com/story/mirai-untold-story-three-young-hackers-web-killing-monster/) provides a detailed interview with the three main authors of this botnet.

IPv6 continues its deployment and operational details matter. An [APNIC blog post](https://blog.apnic.net/2023/11/17/ipv6-the-dns-and-happy-eyeballs/) discusses the importance of supporting IPv6 (and IPv4) on authoritative name servers, and analyzes measurement results. A [blog post](https://majornetwork.net/2023/11/introduction-to-dhcpv6/) provides a brief and interesting overview of the operation of DHCPv6 with sample packet traces to better understand this important protocol.

Major CDNs have started to support Encrypted Client Hello (ECH). This is a TLS extension that enables the encryption of the ClientHello message on TLS session. The main benefit of ECH is that passive observers, such as firewalls, cannot anymore extract the server name from the packets that they process. The deployment of ECH has [several operational implications for network administrators who manage firewalls and security devices](https://blog.apnic.net/2023/11/15/security-control-changes-due-to-tls-encrypted-client-hello/). If you'd like to test and experiment with ECH, the [Guardian Project](https://guardianproject.info/) provides a [detailed tutorial on how to support ECH](https://guardianproject.info/2023/11/10/quick-set-up-guide-for-encrypted-client-hello-ech/) on servers.

During the last years, network operators have deployed RPKI to counter different forms of prefix hijacking using interdomain routing. A growing number of ISPs support this effort. The last addition is [AWS summarized their deployment in a blog post](https://www.manrs.org/2023/10/coordination-key-to-largest-rpki-deployment/).

We are used to the A, AAAA, MX, NS or CNAME DNS records. Recently, the IETF approved [RFC9460](https://www.rfc-editor.org/rfc/rfc9460.html) which defines the SCVB and HTTPS resource records. These records are already used by by large CDNs and web operators. They provide useful information for browsers needing to access remote web sites. An [interesting blog post](https://www.netmeister.org/blog/https-rrs.html) provides a detailed explanation of these two new record types and sample packet traces. 

The last note for this week is the [Packet Run](https://www.sidn.nl/en/news-and-blogs/packet-run-is-an-interactive-marble-run-that-shows-people-how-the-internet-works). This artistic installation at the Dutch Design Week 2023 in Eindhoven is an interactive marble run that shows people how the Internet works. A rare example of art meeting networking... 


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 
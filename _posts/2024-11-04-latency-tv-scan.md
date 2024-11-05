---
layout: post
title: Internet scanning, Latencies, Internet satellites make astronomers angry, Encrypted Client Hello canot be disabled on Chrome, New compression techniques, Cryptography 101, your Smart TV probably spies on you, round robin DNS, OVH failure
tag: scan, privacy, tls, compression, cryptography
author: Olivier Bonaventure
---

Internet scanning is the act of transmitting packets to remote hosts to determine whether they listen on specific protocols or ports. Researchers have used scanning to understand the deployment of new services on the Internet. Attackers have used scanning to detect vulnerable hosts which could be compromised. In the early 2000s, it was difficult to quickly scan the entire IPv4 addressing space. [nmap](https://nmap.org) made it simple for network administrators to detect servers in their network. Nowadays, with software such as [zmap](https://zmap.io), anyone equipped with a 10 Gbps interface can quickly scan the entire IPv4 addressing space for various purposes. A [recent article](https://gsmaragd.github.io/publications/IMC2024/IMC2024.pdf) analyzes the evolution of scanning during the last decade. During this period, Internet scanning has increased 30-fold. Since 2020, the growth is not anymore exponential, but the scans are spread over the entire port range. 

Networking students know that latency is a key performance characteristic in many networks. The [https://cheat.sh/latencies](https://cheat.sh/latencies) website provides a nice visualization of the latencies that every CS student should know. Historical information is also available at [https://colin-scott.github.io/personal_website/research/interactive_latency.html](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

![Latencies that every programmer should know]({{site.baseurl}}/images/Latencies.png)
 Source: cheat.sh


![Latencies to the www.computer-networking.info website]({{site.baseurl}}/images/Geofetcher1.png)


![Latencies to the blog.computer-networking.info web site]({{site.baseurl}}/images/Geofetcher2.png)



Low-orbit satellites such as Starlink, OneWeb, and other constellations provide Internet access services and other telecommunication services. These services can be useful to reach ships or serve areas with low population densities. However, satellite operators aim to serve many more users and deploy many satellites. [Radio astronomers are worried by the interference caused by all these satellites](https://www.astron.nl/starlink-satellites/). Their deployment could block the utilization of different types of radio telescopes.

In TLS 1.3, when a client opens a TLS session, it sends the name of the server in clear in the SNI field of the ClientHello. This field is used by servers and load balancers to determine which service is contacted by the client. Unfortunately, this information can be used to block access to some servers or track the websites visited by users. The IETF is finalizing a technique to encrypt the [TLS Client Hello](https://datatracker.ietf.org/doc/draft-ietf-tls-esni/). Although the RFC has not yet been published, it has already been adopted and deployed. On the browser side, Google has recently announced that this feature is enabled by default on Chrome and that [users will not be able to disable it](https://support.google.com/chrome/thread/260299990/cannot-disable-encrypted-clienthello-in-latest-version-of-chrome-and-edge?hl=en). Enterprises using firewalls that rely on the SNI to filter Internet traffic might be disappointed by this move... On the server side, [Cloudflare has also enabled the support for Encrypted Client Hello](https://blog.cloudflare.com/new-standards).

Various IETF protocols include compression techniques such as [zlib](https://zlib.net) or [brotli](https://www.rfc-editor.org/rfc/rfc7932). [zstandard](https://www.rfc-editor.org/rfc/rfc8878.html) was published in 2021. Cloudflare reports some results with this new compression method in a [blog post](https://blog.cloudflare.com/new-standards). 

Smart devices enable many opportunities to collect more data about humans. A [recent article](https://arxiv.org/abs/2409.06203) analyzed smart televisions from Samsung and LG. These televisions implement a tracking approach called Automatic Content Recognition (ACR). In a nutshell, these televisions capture the image displayed on the TV screen regularly and compare it with a content library. A black-box audit of the ACR network traffic reveals that ACR continues to work if the user connects a PC to the television. The article analyzes the amount of data exchange but does not reveal the information sent by the televisions to the servers operated by Samsung and LG. If you thought your television was dumb... If you think that your smartphone is much safer from a privacy viewpoint, you will probably be worried by this [blog post](mobile privacy https://krebsonsecurity.com/2024/10/the-global-surveillance-free-for-all-in-mobile-ad-data/).


Alfred Menezes, one of the co-authors of the [Handbook of Applied Cryptography](https://cacr.uwaterloo.ca/hac/), will retire in September 2025. One of his post-retirement projects is to develop free, online courses on applied cryptography. He has started to publish the [video lectures](https://cryptography101.ca/crypto101-building-blocks/) of his Crypto 101 course.

[comment]: <>  (https://www.nature.com/articles/d41586-024-03110-0)

Two letters, two-level domain names correspond to internationally recognized countries and territories. The ".us" prefix corresponds to the United States of America, ".tv" to the Polynesian island Tuvalu, and ".io” to the British Indian Ocean Territory. The UK has recently agreed to hand over the control of this territory to Mauritius. Eventually, ISO, which maintains the list of two-letters country codes, will probably remove the ".io" code. It is unlikely that the corresponding domains will disappear, as explained in this [article](https://domainnamewire.com/2024/10/09/io-domain-names-arent-going-away/).

[comment]: <> (fin de OCSP chez Letsencrypt pour des raisons de privacy https://letsencrypt.org/2024/07/23/replacing-ocsp-with-crls/)

[comment]: <> (managing 14k devices using python, lots of info on how to use tcp https://thegraynode.io/posts/pushing_python_to_its_limits/)

An interesting [blog post](https://subseacables.blogspot.com/2024/10/buyer-pricing-guidance-atlantic.html) reveals the cost of 100 Gbps links across the Atlantic. 

Ward Christensen, inventor of BBS and of the [XMODEM protocol](https://en.wikipedia.org/wiki/XMODEM), passed away(https://arstechnica.com/gadgets/2024/10/ward-christensen-bbs-inventor-and-architect-of-our-online-age-dies-at-age-78/) at the age of 78. 

[comment]: <> (US mobile data traffic https://www.rcrwireless.com/20241014/analyst-angle/us-mobile-data-traffic-stats-need-careful-scrutiny-analyst-angle)

[comment]: <> ( https://blog.apnic.net/2024/10/22/the-ipv6-transition/)

The shortest path algorithm invented by Edsger Dijkstra in 1956. It is still a key element of the link-state routing protocols. A [very interesting](https://www.quantamagazine.org/computer-scientists-establish-the-best-way-to-traverse-a-graph-20241025/)  article written by Ben Brubaker for Quanta
provides several interesting insights on this important algorithm. 

Round-robin DNS is a technique that is used by nameservers to return answers for the same request for load balancing or resiliency purposes. An [interesting blog post](https://blog.hyperknot.com/p/understanding-round-robin-dns) analyzes how different browsers interpret these responses.

A [recent study](https://www.opensignal.com/2024/10/31/wi-fi-drives-smartphone-data-consumption-in-the-us-but-trends-vary-across-operators) reveals that, in the USA, most smartphones exchange data using Wi-Fi networks.

At the end of October, OVH, a French cloud provider, suffered from a routing problem. Cloudflare provides [some information](https://blog.cloudflare.com/cloudflare-perspective-of-the-october-30-2024-ovhcloud-outage) about the reasons for this outage. Shortly after, Octave Klaba, OVH's founder and CEO, confirmed that the problem was caused by one of their upstreams, WorldStream, announcing too many prefixes. 

![OVH deploys max prefix]({{site.baseurl}}/images/Ovh-maxprefix.png)

[comment]: <> (XFRM IPSec in Linux https://pchaigno.github.io/ebpf/2024/10/30/linux-xfrm-ipsec-reference-guide.html)

The eBPF instruction set, which enables developers to insert code inside the Linux kernel, is now documented in [RFC9669](https://www.rfc-editor.org/rfc/rfc9669.html). Thanks to the eBPF virtual machine, the Linux kernel is more flexible and more extensible. This technique has also been proposed to [extend Internet protocols](https://pluginized-protocols.org/2020/10/01/success.html).




This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
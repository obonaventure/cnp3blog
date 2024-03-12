---
layout: post
title: Improving TCP performance, eBPF in the Linux kernel, sending packets faster in go, time to repair undersea cables, TLS certificates, netlab and the NTP pool
tag: tcp, ebpf, linux, performance, netlab, ntp, tls
author: Olivier Bonaventure
---

[TCP](https://beta.computer-networking.info/syllabus/default/protocols/tcp.html) remains the most widely used reliable transport protocol. During the last years, [QUIC](https://www.rfc-editor.org/rfc/rfc9000.html) has started to replace TLS over TCP for some applications such as HTTP/3. This is not the first time that new transport protocols are proposed to replace TCP. During the late 1980s, XTP and similar protocols aimed at being faster than TCP that was already to be considered as an old protocol.  David Clark, Van Jacobson and their colleagues [showed that TCP implementations could run much faster](https://ieeexplore.ieee.org/abstract/document/29545). They introduced a fast path, i.e. a part of the TCP implementation where the stack is optimized to process the next packet if it arrives in sequence. Improving the performance of TCP implementations is an ongoing effort. A [recent set of patches proposed by Coco Li for the Linux kernel achieves 30-40% performance improvements](https://www.phoronix.com/news/Linux-6.8-Networking) on AMD processors by better exploiting their caches.

The Linux kernel is a key operating system for the Internet since it is used on servers, routers and smartphones. This Linux kernel continues to evolve. During the last years, the Linux kernel gained a lot of flexibility thanks to the addition of the eBPF virtual machine that can execute user-supplied programs directly inside the kernel. This allows to monitor various kernel components, tune various algorithms and provide advanced functions in the networking stack and elsewhere. A recent [documentary](https://www.brendangregg.com/blog/2024-03-10/ebpf-documentary.html) explains the evolution of eBPF since 2014 and its main usages.

[Andree Toonk](https://toonk.io) explores in a [blog post](https://toonk.io/sending-network-packets-in-go/index.html) the different techniques that allow to send packets quickly in [go](https://go.dev). One the techniques he discusses uses eBPF programs to send packets.

Last week, three optical fibers were damaged in the Red Sea. This forced Internet providers to reroute traffic between Marseille and Singapore over different paths. An [interesting article](https://www.capacitymedia.com/article/2cxld9zwx6s3ouijwml1c/news/how-is-subsea-traffic-being-rerouted-after-red-sea-cable-cuts) discusses the time to repair the damages on undersea cables.

[TLS](https://beta.computer-networking.info/syllabus/default/protocols/tls.html) certificates were initially only distributed by certification authorities that charged a lot of money for each certificate. Fortunately, the non-profit [Let's Encrypt](https://letsencrypt.org) certification authority democratized the utilization of TLS certificates. Nowadays, any server administrator can easily obtain TLS certificates. However, this still requires installing and configuring software that supports the [ACME protocol](https://en.wikipedia.org/wiki/Automatic_Certificate_Management_Environment). The [EFF](https://www.eff.org) discusses in a blog post [possible next steps](https://www.eff.org/deeplinks/2024/03/should-caddy-and-traefik-replace-certbot) such as including these modules directly in popular web servers.

The [Web Check](https://web-check.xyz) provides an open-source set of checks that can be launched on a web server to verify several dozen aspects of its configuration, including TLS, DNS records, ... A good starting point to explore the configuration of web sites.


![Web check]({{site.baseurl}}/images/WebCheck.png)

[netlab](https://netlab.tools) is a set of python modules that allow to build virtual network labs. netlab supports images from various commercial router vendors. The latest version has [added support for open-source daemons](https://blog.ipspace.net/2024/03/netlab-1-8-0-daemons-bird.html) including [Bird](https://bird.network.cz) or DNSMasq. Another [blog post](https://blog.ipspace.net/2024/03/netlab-cyber-crane-mesh-lab.html) reports that netlab can emulate networks with up to 50 devices on a server equipped with 128 GB of RAM and 32 CPU cores...


For satellite-based access networks like Starlink, latency is an important concern. The Starlink engineers have managed to significantly reduce the latency of their commercial services by tuning the configuration of their network. A [technical report](https://api.starlink.com/public-files/StarlinkLatency.pdf) provides some additional information about this change.

![Starlink latency]({{site.baseurl}}/images/Starlink-latency.png)

The [NTP pool project](https://pool.ntp.org) manages more than 4k NTP servers to provide time synchronization services to anyone. A [recent scientific article](https://gsmaragd.github.io/publications/SIGMETRICS2024/) analyzes a lot of measurements about this important service.

![NTP Pool]({{site.baseurl}}/images/Ntppool.png)



This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
---
layout: post
title: TCP on planes and highspeed trains
tag: TCP, MPTCP, QUIC, planes, trains
author: Olivier Bonaventure
---


Internet protocols continue to be used in a variety of scenarios that go beyond the initial objectives of the TCP/IP protocol suite. Two recent scientific articles provide an insight at the performance of TCP/IP in challenging environments.

In [Mile High WiFi:A First Look At In-Flight Internet Connectivity] (http://www.cs.northwestern.edu/~jpr123/papers/www-flight.pdf), John Rula and his colleagues analyse the performance of Internet protocols during flight on commercial planes. They spent more than 45 hours in planes and measured the quality of the supplied Internet access. They compare two different access technologies, namely [Mobile Satellite Service (MSS)](https://en.wikipedia.org/wiki/Mobile-satellite_service) and [Direct Air to Ground Communication (DA2GC)](https://www.etsi.org/deliver/etsi_tr/103100_103199/103108/01.01.01_60/tr_103108v010101p.pdf). These two technologies exhibit higher latencies (200 ms on average for DA2GC and 750 ms for MSS) and much higher loss rates that those of the fixed Internet. They analyse the performance of HTTP and QUIC in details.

In [A Measurement Study on Multi-path TCP with Multiple Cellular Carriers on High-speed Rails](https://www.cs.purdue.edu/homes/chunyi/pubs/sigcomm18-li.pdf), Li Li et al. look at a very different scenarios. After Europe, China has now deployed various high-speed trains. The authors analyse in details the performance of TCP in such trains. More precisely, they study the benefits that [Multipath TCP](https://www.multipath-tcp.org) can bring by enabling endhosts to simultaneously use two cellular networks. Given the speed of these trains, cell handovers are very frequent and by combining two different cellular providers, it becomes possible with [Multipath TCP](https://www.multipath-tcp.org) to improve the performance. The detailed measurements indicate that Multipath TCP is more robust than TCP, but improvements are still possible.

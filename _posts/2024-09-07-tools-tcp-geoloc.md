---
layout: post
title: Networking tools for students, post quantum cryptography, observing TCP on a large CDN, transient DNS domain names, IP geolocation
tag: tools, cryptography, tcp, geolocation
author: Olivier Bonaventure
---


Networking students need to understand how protocols operate, but also learn to use various tools. [Julia Evans](https://wizardzines.com/) provides interesting wizard zines on various networking and Linux related topics. She also has a nice poster showing the list of networking tools that she uses. Can you also efficiently use these networking tools ?


![Julia Evans list of networking tools]({{site.baseurl}}/images/B0rk-tools.png)

Today's secure protocols like ssh or TLS 1.3 rely on cryptographic algorithms such as RSA, AES, or Chacha. The progress in quantum computing threatens the security of some of these algorithms. Cryptographers have been searching for algorithms that are resistant against attacks from quantum computers for many years. Several years ago, the NIST issued a call for post-quantum algorithms to support encryption and digital signatures. After a very long review process, [NIST has standardized three algorithms, a fourth one will be standardized later this year](https://spectrum.ieee.org/post-quantum-cryptography-2668949802). Several companies have already started to use [post-quantum cryptography in TLS](https://blog.cloudflare.com/pq-2024). If you use Firefox, the [pqspy addon](https://addons.mozilla.org/en-GB/firefox/addon/pqspy/) allows you to check which websites use Post Quantum cryptography. The [source code of the addon is available on GitHub](https://github.com/bwesterb/pqspy).

Cloudflare is a large CDN that handles about 60 millions HTTP requests per second globally. Some of these HTTP requests are not created by humans who visit popular websites. While serving these requests, they also log a lot of information about the established TCP connections. In a [very interesting and detailed blog post](https://blog.cloudflare.com/tcp-resets-timeouts), they explore the different types of TCP RST that they receive an infer several types of strange behaviors from different sources.  

Geolocation, the ability to determine the geographical coordinates of an IP address, is an important feature for many services. Advertisers rely on geolocation to tune the advertisements on websites, but some firewalls rely on geolocation to block access to some clients. The Internet was not designed with geolocation in mind and various solutions have been proposed to geolocate IP address. A simple approach is to look at whois records and extract the address of the company who registered each prefix. For large ISPs, this often means that most of its IP addresses are geolocated at the ISP's headquarters. Over the years, other approaches have been proposed. The IETF adopted a pragmatic approach in [RFC9632](https://www.ietf.org/rfc/rfc9632.html). It enables ISPs and enterprises to easily publish the geolocation of their IP addresses. There are already almost 300k prefixes covered by this approach. You can explore it from [http://geolocatemuch.com/](http://geolocatemuch.com/).


An [interesting measurement study](https://arxiv.org/pdf/2405.12010) looks at the DNS domain names that are registered and then unregistered within less than 24 hours. Some of these transient domains are probably due to typos, but most of them are likely related to fraudulent activities. The researchers have counted more than 20,000 transient domains during January 2024 only.

While readers of this blog probably take the Internet as granted for everybody, a [recent article](https://www.theregister.com/2024/09/03/three_billion_people_offline/) reminds us that more than 3 billion inhabitants of this planet cannot use the Internet. The article discusses the efforts of GSMA to promote low cost phones and those of the Internet Society.
 

This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
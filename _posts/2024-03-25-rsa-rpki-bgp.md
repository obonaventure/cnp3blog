---
layout: post
title: BGP technology deep dive, 50% of RPKI valid routes, 2048 bits RSA keys are deprecated, slow web sites and an open letter from Tim Berners-Lee
tag: bgp, rpki, rsa, web, cryptography
author: Olivier Bonaventure
---

The [IETF](https://www.ietf.org) met last week in Brisbane, Australia, for its [119th meeting](https://www.ietf.org/how/meetings/119/). In addition to the IETF working groups meetings, there were two interesting one-hour sessions on BGP:

 - [An introduction to the Border Gateway Protocol for protocol developers](https://youtu.be/dGuNeZk5PNA)
 - [Extending BGP: Reviewing the past and exploring the future](https://youtu.be/xf0SNZrr_9g)

![BGP swiss army knife]({{site.baseurl}}/images/Ietf119-bgp1.png)


BGP has a wide range of use cases and the second part of the technology deep dive for those willing to understand the new applications that are build above BGP.

One of the current operational challenges for network operators is to deploy the RPKI. According to [cloudflare's radar](https://radar.cloudflare.com/routing), [half of the BGP are now RPKI valid](https://x.com/heymingwei/status/1771193361224863843?s=61&t=tP_GnIXAFXsfnvztGctEig).

![50% RPKI valid BGP routes]({{site.baseurl}}/images/rpki50.png)


Microsoft [has announced](https://learn.microsoft.com/en-us/windows/whats-new/deprecated-features) that Windows client will not anymore accept TLS server certificates with an RSA key shorter than 2048 bits.

On X, Bas Westbaan provided a photo of a [slide showing that the X25519 Elliptic Curve used for the EC-DH key agreement was consuming 0.05% of the CPU on meta's datacenters](https://x.com/bwesterb/status/1771958142147973390?s=61&t=tP_GnIXAFXsfnvztGctEig). Security has some non-negligible CPU cost...

![X25519 CPU cost at meta]({{site.baseurl}}/images/meta-x25519.png)

The average size of web pages continues to grow. This affects user with low bandwidth connections who need to download a large amount of images, video or JavaScript to view a single web page. A [recent study](https://danluu.com/slow-device/) shows that this also affects the users who do not use recent laptops or smartphones. If you design web sites, please take them into account when you decide to add new fancy features...

![Web on slow CPUs]({{site.baseurl}}/images/slow-web.png)


A few weeks ago, [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) published an [open letter](https://medium.com/@timberners_lee/marking-the-webs-35th-birthday-an-open-letter-ebb410cc7d42) where he shares his concerns about the centralization of the web and the fact that it is often used to collect personal data. Hopefully, the adoption of the [Contract for the web](https://contractfortheweb.org) will allow the web to improve in the coming years...

This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
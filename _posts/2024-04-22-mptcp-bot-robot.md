---
layout: post
title: Multipath TCP in the Linux kernel,  beyond robots.txt, web performance, high frequency trading and the communication network of the Tudors
tag: mptcp, bot, ai, privacy, history, high frequency trading
author: Olivier Bonaventure
---


The implementation of [Multipath TCP](https://www.rfc-editor.org/rfc/rfc8684.html) continues to be improved in the mainstream Linux kernel. With Multipath TCP, the packets that belong to a single connection can be exchanged over different paths. For example, a smartphone can use both Wi-Fi and cellular for a single connection. This enables seamless handovers for mobile applications and improves user experience. With Multipath TCP, a TCP can use both IPv4 and IPv6 and dynamically select the best performing address family in real time. If you have not yet enabled Multipath TCP on your Linux hosts or servers, check [https://www.mptcp.dev](https://www.mptcp.dev), the website of the Multipath TCP maintainers in the Linux kernel.

Many web servers use the [robots.txt](https://www.rfc-editor.org/rfc/rfc9309.html) file to indicate which part of their web site should be indexed by search engines and which part should be ignored. With the recent AI tools like ChatGPT that retrieve billions of web pages to train their models, some webmasters want to use a similar approach to restrict the part of their servers which can be downloaded to train AI models. [Mark Nottigham](https://www.mnot.net/) discusses this problem in a recent [blog post](https://www.mnot.net/blog/2024/04/21/ai-control). A [recent study](https://www.imperva.com/company/press_releases/bots-make-up-half-of-all-internet-traffic-globally/) shows that bots, either AI/search related bots or fraudulent bots trying to exploit vulnerabilities constitute a growing fraction of the total Internet traffic. As reported on [X](https://x.com/chris_j_paxton/status/1778441113709281667?s=61&t=tP_GnIXAFXsfnvztGctEig) some of these bots can be stuck on websites like [http://www.web.sp.am](http://www.web.sp.am) that try to lurk spammers...


![web.sp.am]({{site.baseurl}}/images/spider.png)

In parallel, there is an [ongoing effort](https://www.privacytxt.dev) to develop a define a machine readable format to define the privacy policy of websites : privacy.txt. It will be interesting to analyze the adoption of this new file format.

A [recent scientific study](https://arxiv.org/abs/2308.06409) looked at Google CrUX data to compare the performance of Internet access in different European countries. This is a nice approach to look at Internet performance from the user's viewpoint.

High Frequency Trading is a service that enables banks to quickly buy and sell shares on the stock market. This is a very demanding application from a networking viewpoint since a few microseconds of delay can make a huge difference in the cost of the shares that are exchanged. An interesting interview posted on [youtube](https://www.youtube.com/watch?v=42AUkkN_nBs) provides more information on the networking aspects of HFT. 


Source address spoofing remains possible in some parts of the Internet. In most cases, when attackers send spoof packets, they use almost random source addresses to hide their activities. A [recent analysis](https://blog.apnic.net/2024/04/19/destination-adjacent-source-address-spoofing/) reveals that some spoofers now use as source address an address that is very close to their target. 

To conclude this post, I encourage you to explore [https://tudornetworks.net/](https://tudornetworks.net/). This is a nice visualization of the 123,850 letters exchanged by the Tudor government between from the accession of Henry VIII to the death of Elizabeth I (1509-1603). This network was rich of 20,424 people. 

![Tudor]({{site.baseurl}}/images/tudor.png)


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
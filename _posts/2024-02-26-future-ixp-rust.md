---
layout: post
title: Memory safety, bloating web pages with JavaScript, future Internet eXchange Points, securing instant messaging and troubleshooting techniques
tag: security, cryptography, javascript, IXP
author: Olivier Bonaventure
---

A large fraction of the client and servers that we used on the Internet are programmed using the C or C++ programming languages. These languages allow programmers to interact with the low-level hardware and achieve good performance. Unfortunately, small errors in the handling of memory have caused huge security problems. More recent programming languages like [Rust](https://www.rust-lang.org) provide stronger protection when accessing memory and allow to avoid a range of security problems. The White House has published a [report](https://www.whitehouse.gov/oncd/briefing-room/2024/02/26/press-release-technical-report/) that encourages the entire software industry to adopt techniques to design and implement memory safe programs.


Niki Tonsky looked in an interesting [blog post](https://tonsky.me/blog/js-bloat/) at the size of the JavaScript code that different web pages load by default. Wikipedia only loads 200 KB of JavaScript, but other web pages can force you to load up to several tens of MBs of JavaScript only to access their front page. Even if many of these JavaScript codes are shared by different web sites, it is unclear why a single web page needs more code than entire programs and even operating systems several years ago.

Internet eXchange Points (IXPs) play an important role in today's Internet by allowing many ISPs to peer efficiently at multiple locations. Thomas King, the CTO of DE-CIX, a German IXP, shares [his reflections](https://www.networkcomputing.com/cloud-infrastructure/internet-exchange-future-scalable-automated-secure) on the evolution of IXPs in the coming years. He envisions robots to automatically connect router cards, more automation, improved scalability, resilience and security and expects that more enterprises will use IXPs directly.

Many users rely on instant messaging to exchange information with friends, family and colleagues. These applications provide different levels of security.

![Security in messaging applications]({{site.baseurl}}/images/Imessage-pq3.png)

Apple has recently updated the cryptographic techniques used to ensure the security of the messages exchanged using iMessage. They provide a detailed overview of the new deployed technique in a [blog post](https://www.networkcomputing.com/cloud-infrastructure/internet-exchange-future-scalable-automated-secure). The new technique allows to counter attackers that are able to collect encrypted messages, store them, possibly for several years, in the hope that new techniques such as quantum computers, will allow to decrypt these messages. The security of the new technique has been proven using formal methods, which is relatively rare in the Internet industry that usually focuses on shipping possibly unfinished products.

Network engineers often need to troubleshoot bizarre problems in the networks they manage. Like plumbers, they often improve their troubleshooting skills on the field by solving different types of problems. Students rarely manage real networks and it is difficult for them to acquire troubleshooting skills that are required by industry. Brandon Hitzel has collected a nice set of troubleshooting examples in a [blog post](https://www.networkdefenseblog.com/post/network-troubleshooting-tips).


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
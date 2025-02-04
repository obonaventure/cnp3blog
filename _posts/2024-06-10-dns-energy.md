---
layout: post
title: Slow Internet, DMARC and co, DNS4ALL, a simple DNS resolver
tag: internet, email, ping, traceroute, dns, energy
author: Olivier Bonaventure
---

A periodical reminder for protocol or web site designers. While many of these designers use a recent computer with a good or very good Internet connection, some of their users rely on low performance Internet connection or older computers. If you can optimize your protocol or website for these users as well, every body will benefit. A recent [blog post](https://brr.fyi/posts/engineering-for-slow-internet) discusses this problem from the viewpoint of a researcher working in Antarctica, but there are many locations with a less powerful Internet connection on Earth.


While email delivery still relies on the venerable SMTP protocol, modern mail software use techniques that include DMARC, SPF and DKIM. The [leardmarc.com](https://www.learndmarc.com/) website provides an interactive visualization of how these checks are performed by mail servers based on one test email that you send. An interesting way to check how your mail servers (or those of your email provider since managing your own mail server remains difficult).


![learndmarc.com]({{site.baseurl}}/images/dmarc.png)

You probably use the DNS resolver of your ISP or university to resolve the domain names that your applications use. During the last years, large cloud providers such as google or Cloudflare have deployed their own DNS resolvers and encouraged users to switch to their resolvers. [SIDN Labs](https://www.sidnlabs.nl) has deployed a [new anycast set of DNS resolvers](https://www.sidnlabs.nl/en/news-and-blogs/dns4all-sidn-labs-experimental-public-dns-resolver) under the name DNS4ALL. These resolvers use the [unbound DNS resolver](https://nlnetlabs.nl/projects/unbound/about/). This is set of open DNS resolvers will be used to collect measurements. See [https://dns4all.eu](https://dns4all.eu) for additional information.

The [APNIC blog](https://blog.apnic.net) published an interesting [post](https://blog.apnic.net/2024/06/12/lessons-learned-when-building-my-dns-resolver/) by Wen-Tsung (Bill) Chang that describes his experience in building a simple DNS resolver in rust from scratch. A nice way to learn all the   details about implementing real Internet protocols.

Network operators are more concerned about the energy consumed by their networks. Router vendors are making progress in tuning their equipment to use less energy. In two interesting videos, Nicolas Fevrier analyzes the power consumption of Juniper routers and how it can be optimized. The [first video](https://www.youtube.com/watch?v=-aEYNDRB9Ck) discusses the main factors that influence the power consumption of ISP routers. The [second video](https://www.youtube.com/watch?v=TNDoGGC0jqk) describes how operators can configure some of their routers to use less energy. There is a bit of marketing in this video, but it provides real numbers that are interesting.






This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
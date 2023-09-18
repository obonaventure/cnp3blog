---
layout: post
title: ECMP, IPv6 DNS and BGP failures
tag: ecmp, ipv6, dns, bgp, failures
author: Olivier Bonaventure
---

[Equal Cost Multipath (ECMP)](https://www.rfc-editor.org/rfc/rfc2992) is a a key feature of IP networks compared to other types of networks. With ECMP, routers can send the packets belonging to different flows over different paths provided that these paths have the same cost. This technique is widely used to spread the load in large networks. [Dip Sing](https://dipsingh.github.io/) describes with a lot of details in a [recent blog post](https://dipsingh.github.io/Flow-Distribution-Across-ECMP/) how routers spread packets from different flows over different paths.

[Dan Wing](https://www.employees.org/~dwing/) regularly collects [data](https://www.employees.org/~dwing/aaaa-stats/) about the DNS AAAA records that are advertised by the top domain names. Interesting source of information to track the deployment of IPv6. The statistics also reveal some configuration errors. [Julia Evans](https://jvns.ca) has started a [nice project](https://github.com/jvns/dns-doctor) that allows to verify whether a DNS configuration is correct. This would work well to validate student labs, but also for top DNS names apparently.

The [Border Gateway Protocol (BGP)](https://beta.computer-networking.info/syllabus/default/protocols/bgp.html?highlight=bgp) is probably the most important routing protocol today. When BGP fails, it can lead to lots of disruptions on the global Internet. [Doug Madory](https://www.kentik.com/blog/author/doug-madory/) has summarized in a recent blog post [a brief history of the Internet's biggest BGP incidents](https://nanog.org/stories/articles/a-brief-history-of-the-internets-biggest-bgp-incidents/). Very useful for students who start their exploration of BGP.


You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). 
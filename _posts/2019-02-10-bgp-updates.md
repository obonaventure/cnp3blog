---
layout: post
title: Recent BGP news
tag: bgp
author: Olivier Bonaventure
---


The [Border Gateway Protocol (BGP)](http://cnp3book.info.ucl.ac.be/2nd/html/protocols/bgp.html) is probably the most important routing protocol in today's Internet. Its main role is the exchange of interdomain routes, but it also plays a key role inside ISP networks to support various services. 
This post provides pointers to recent articles and blog posts that are directly related to the evolution of BGP and could be of interest to the readers of [Computer Networking : Principles, Protocols and Practice](http://cnp3book.info.ucl.ac.be).

Geoff Huston continued his tradition of summarizing the evolution of the BGP routing tables at the beginning of each year. His [recent blog post](https://blog.apnic.net/2019/01/16/bgp-in-2018-the-bgp-table/) summarizes what he observed during the last year and his predictions for the future. Looking at the IPv6 BGP routing tables, there is a risk entering an exponential growth in the coming years given the growing fragmentation of the announced IPv6 prefixes.

In December 2018, a group of researchers announced a [plan](https://mailman.nanog.org/pipermail/nanog/2018-December/098458.html) to conduct some experiments by announcing a route for prefix [184.164.224.0/24](https://stat.ripe.net/184.164.224.0%2F24#tabId=at-a-glance) with specific BGP attributes to test the feasibility of a new technique to improve the security of BGP routes. BGP, like many Internet protocols, is extensible and BGP implementations can negotiate the utilisation of new BGP attributes. Unfortunately, [past experience](https://labs.ripe.net/Members/erik/ripe-ncc-and-duke-university-bgp-experiment) has shown that announcing new BGP attributes could disrupt interdomain routing by triggering bugs in existing BGP implementation. In 2010, such an experiment [triggered bugs in Cisco routers](https://labs.ripe.net/Members/erik/ripe-ncc-and-duke-university-bgp-experiment) and caused major disruptions. In January 2019, the new experiment, [triggered a bug in FRRouting](https://mailman.nanog.org/pipermail/nanog/2019-January/098761.html), a popular open-source implementation of BGP. [FRRouting](https://frrouting.org) has since been [updated](https://lists.frrouting.org/pipermail/frog/2019-January/000404.html). Apparently, [some ISPs were affected by this experiment](https://www.zdnet.com/google-amp/article/internet-experiment-goes-wrong-takes-down-a-bunch-of-linux-routers/). This shows that unfortunately it remains very difficult to extend BGP in 2019.

On the more positive side, the security of BGP continues to slowly improve with the efforts of [MANRS](https://www.manrs.org) and some operators. In a recent [blog post](https://www.manrs.org/2019/02/routing-security-getting-better-but-no-reason-to-rest/), Andrei Robalchevsky summarized the security problems that affected BGP in 2018. There seems to be a postive trends when comparing 2017 and 2018. [Ben Cox](https://github.com/benjojo) also summarized his views on the [evolution of the deployment of the RPKI](https://blog.benjojo.co.uk/post/state-of-rpki-in-2018).

Another interesting blog post was [Ben Cox](https://github.com/benjojo) who described how he configured lightweight virtual machines to run BGP in order to [simulate the London tube by using BGP with the MED attribute](https://blog.benjojo.co.uk/post/eve-online-bgp-internet). 


*This blog post was written to inform the readers of [Computer Networking : Principles, Protocols and Practice](http://cnp3book.info.ucl.ac.be) about the evolution of the field. You can subscribe to the Atom feed for this blog at [https://obonaventure.github.io/cnp3blog/feed.xml](https://obonaventure.github.io/cnp3blog/feed.xml).*

---
layout: post
title: Disconnecting an entire country from the Internet
tag: bgp
author: Olivier Bonaventure
---

In the early days of the Internet, governments considered it as a strange
research experiment and did not really bother to try to understand how it worked. During the last years, governments all over the world have made more and more efforts to try to control its utilization. The numbers of laws that affect the Internet consider to grow and various governments restrict the utilization of the Internet. It is impossible to list all government interferences that affect the Internet, but here are a few notable ones.

The most recent one is that Russia announced that they have a plan to test the [impact of being disconnected from the Internet](https://www.zdnet.com/article/russia-to-disconnect-from-the-internet-as-part-of-a-planned-test/). This is the first time that such a large country tries to evaluate the impact of being disconnected from the Internet. According to a [ZDNet article](https://www.zdnet.com/article/russia-to-disconnect-from-the-internet-as-part-of-a-planned-test/) this test is designed to ensure *the independence of the Russian Internet space (Runet) in the case of foreign aggression*. Apparently, the test will occur before April 1st, we'll see its outcome and the lessons that Russia will learn from it in the coming weeks. 

An interesting point to observe during this test will be how BGP and DNS reacts to such a large and coordinated disconnection. For DNS, most of the [root servers](https://www.iana.org/domains/root/servers) are managed by organizations outside of Russia. Some of them are distributed are there are replicas of
the [K](https://k.root-servers.org), [L](https://l.root-servers.org), [J](https://j.root-servers.org) and [F](https://f.root-servers.org) root servers near Moscow and a few others in other parts of Russia. BGP routers from Russia and interconnected with other BGP routers from all over the world, disconnecting all of them will result in a large number of BGP update messages.

This is not the first time that a country decides to disconnect itself from the Internet. [Iraq](https://blogs.oracle.com/internetintelligence/iraq-downs-internet-to-combat-cheating-again), [Syria](https://twitter.com/InternetIntel/status/870278975716831232) and [Ethiopia](https://globalvoices.org/2017/06/01/ethiopia-imposes-nationwide-internet-blackout/) regularly block the Internet or parts of it during national school exams. [Algeria](https://www.middleeastmonitor.com/20180614-algeria-cutting-off-internet-during-high-school-exams/) used a similar approach to prevent cheating.

Some poorly connected countries can be disconnected from the Internet due to the failure of a fiber optic link. This happened last year in the Democratic Republic of Congo, in Ghana. The [Internet Intelligence map](https://map.internetintel.oracle.com/) collects these events automatically. More details can usually be found on their [blog](https://internetintel.oracle.com/blogs.html).

*This blog post was written to inform the readers of [Computer Networking : Principles, Protocols and Practice](http://cnp3book.info.ucl.ac.be) about the evolution of the field. You can subscribe to the Atom feed for this blog at [https://obonaventure.github.io/cnp3blog/feed.xml](https://obonaventure.github.io/cnp3blog/feed.xml).*

---
layout: post
title: Networking notes for July 2026
tag: tls, email, http, cryptography, time, 5G
author: Olivier Bonaventure
---

Researchers at Foundation Bruno Kessler in Italy have collected 50 million TLS sessions established by computers in their institution to analyze the different versions of TLS that are currently used. Their [technical report](https://arxiv.org/pdf/2605.31020) provides interesting insights on the deployed TLS implementations.

Interesting analysis of the [public peerings](https://labs.ripe.net/author/gael-hernandez/peering-capacity-at-public-internet-exchanges-what-the-data-reveals/) deployed at IXPs. Akamai leads with 79 Tbps of capacity, three times more than Google.

Nice [animation](https://www.linkedin.com/posts/luke-kehoe_starlinks-usage-footprint-has-shifted-from-ugcPost-7468649530876424193-uIQ4/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAA_wFcBtrzDYok3TgEsdsGk9Lnn1hU_xlI) showing the deployment of Starlink terminals in Europe during the last five years. 

A very detailed discussion on [how to setup your own email server](https://anil.recoil.org/notes/recoil-self-hosting-2026). Over the years, with the growth of spam and other email related problems, running an email server has been more and more difficult. As the Internet is a decentralized system, this is a very good exercise for students willing to learn or to operate real production servers. 

The Unicode consortium regularly discusses proposals for new characters. This includes various emojis. Charlotte Eiffel Lilith Buff  maintains a [list of all rejected emojis](https://charlottebuff.com/unicode/misc/rejected-emoji-proposals/).

[RFC10008](https://www.rfc-editor.org/info/rfc10008/) describes the new QUERY method for the HTTP protocol. This method should in the coming years replace the existing usage of GET or POST to send a request with queried values to a specific server. This is also the first published RFC whose number is above 10000!

[RFC9958](https://www.rfc-editor.org/info/rfc9958/) provides a clear description of post quantum cryptography for network and protocol engineers.

[Interesting discussion](https://spectrum.ieee.org/digital-surveillance) in IEEE Spectrum with various examples showing how our smartphones, connected cars and other devices collect data that is often requested by police officers.

In the USA, 5G Fixed Wireless Access (FWA) already serves more than 14 million customers. This becomes a widespread Internet access technology, next to xDSL, cable, fiber and satellite.

A problem with the [time service](https://www.theguardian.com/business/2026/jul/10/telstra-ceo-deeply-sorry-for-outage-and-admits-risk-of-time-keeping-failure-was-known-ntwnfb) caused a huge outage in the Telstra network in Australia when it went back in time to November 2006.

Doug Madory provides [a closer look](https://www.kentik.com/blog/what-the-world-cup-looks-like-in-internet-traffic/) at the impact of World Cup football matches on Internet traffic.


This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
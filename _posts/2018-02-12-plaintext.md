---
layout: post
title: The end of plaintext protocols ?
tag: http, https
author: Olivier Bonaventure
---

Internet protocols have traditionally been clear-text protocols and many protocols
like SMTP or HTTP could be tested by using a simple telnet session. This feature was
very handy when testing or debugging protocol implementations. However, it is
difficult to implement a correct parser for plaintext protocols and many of these parsers 
have suffered from bugs. Binary protocols have a more precise syntax and are thus
easier to parse at least if they do not contain lots of extensibility. All Internet security
protocols including IPSec, TLS or ssh are binary protocols. With the Snowden revelations,
the IETF has strongly encouraged the utilisation of security protocols to counter
pervasive monitoring as explained in [RFC7258](https://datatracker.ietf.org/doc/rfc7258/).

This has resulted in a very fast growth of the utilisation of security protocols such as TLS. 
Google's [transparency report](https://transparencyreport.google.com/https/overview?hl=en) shows that in 
early 2018, Chrome users use HTTPS to interact with more than 2/3 of the existing web
sites.

![https://transparencyreport.google.com/https/overview?hl=en]({{ site.baseurl }}/images/https-stats.png)

This fast growths of HTTPS is likely a consequence of several factors. The Snowden revelations
have encouraged more system administrators to enable HTTPS on their servers. This large scale
deployment has been made possible by several factors. First, with the growth in CPU
performance and the implementation of more efficient cryptographic algorithms, the overhead
of supporting HTTPS as decreased. A detailed discussion of these improvements may be 
found in a recent posting by Vlad Krasnov on [Cloudflare's blog](https://blog.cloudflare.com/how-expensive-is-crypto-anyway/amp/?__twitter_impression=true). A second factor are the TLS certificates. A few years ago, 
installing TLS certificates on a server was both lengthy and cumbersome. This
deployment problem has been illustrated in the [“I Have No Idea What I’m Doing” -
On the Usability of Deploying HTTPS](https://www.usenix.org/conference/usenixsecurity17/technical-sessions/presentation/krombholz) paper presented at Usenix Security. Fortunately,
the [Let's Encrypt](https://letsencrypt.org/) project developed an excellent
and automated method to associate TLS certificates to web sites. In less than
two years, they have managed to distribute more than 70 millions of certificates, 
a huge achievement from a deployment viewpoint.

![https://letsencrypt.org/stats/#growth]({{ site.baseurl }}/images/letsencrypt.png)

This growth of HTTPS is far from being finished. Recently, Google has [announced](https://security.googleblog.com/2018/02/a-secure-web-is-here-to-stay.html) that in June 2018,
Chrome will flag the web sites that do not support HTTPS as insecure. Firefox 59+ also
contains a [configuration parameter](https://blog.cloudflare.com/https-or-bust-chromes-plan-to-label-sites-as-not-secure/amp/?__twitter_impression=true) to flag HTTP websites as being not secure.

In a [recent post](https://blog.cloudflare.com/https-or-bust-chromes-plan-to-label-sites-as-not-secure/amp/?__twitter_impression=true) on Cloudflare's blog, Patrick Donahue provides an interesting summary
of the history of the deployment of HTTPS. 


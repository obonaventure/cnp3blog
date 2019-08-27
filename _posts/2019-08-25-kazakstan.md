---
layout: post
title: A Man in the Middle in Kazakhstan
tag: tls, censorship
author: Olivier Bonaventure
---

In less than 20 years, [Transport Layer Security (TLS)](https://www.computer-networking.info/2nd/html/protocols/tls.html) moved from a niche protocol that was only used by banks and e-commerce websites to almost a default solution. Today, a growing fraction of the Internet traffic is encrypted by using TLS or similar protocols. This encryption and the associated authentication improve the security of Internet users since attackers cannot observe or modify the packets that they exchange.

Another consequence of the widespread deployment of encryption is that it becomes more difficult for the police, secret services and other agencies to collect that data exchanged between users. Some non-democratic countries where surveillance is widespread have attempted to circumvent this by deploying technical solutions that enable them to decrypt (parts of) the Internet traffic. The last public known example occurred in Kazakhstan during the summer.

On July 18th, 2019, a [bug report](https://bugzilla.mozilla.org/show_bug.cgi?id=1567114) was filed on Mozilla's bug tracker.


![Bugzilla Kasakhstan]({{ site.baseurl }}/images/bugzilla.png)

In a nutshell, users in Kazakhstan were forced to install a government-issued root-certificate that enabled them to place "transparent" proxies on all TLS traffic to decrypt it.

This bug report initiated a lot of discussions among browser implementors and activists. A detailed analysis of this HTTPS interception was published by Ram Sundara Raman et al on a [blog](https://censoredplanet.org/kazakhstan). This is not the first time that the Kazakh government tried to deploy its own root certificate as noted by Catalin Cimpanu in a [ZDnet article](https://www.zdnet.com/google-amp/article/kazakhstan-government-is-now-intercepting-all-https-traffic/?__twitter_impression=true).

Within a month, three key browser vendors: Mozilla, Google and Apple [patched their software](https://www.zdnet.com/article/apple-google-and-mozilla-block-kazakhstans-https-intercepting-certificate/) to ban the Kazakh's root certificate from their browsers. Both Google and Mozilla explained their decision in dedicated blog posts [1](https://security.googleblog.com/2019/08/protecting-chrome-users-in-kazakhstan.html?m=1) [2](https://blog.mozilla.org/blog/2019/08/21/mozilla-takes-action-to-protect-users-in-kazakhstan/) 




*This blog post was written to inform the readers of [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) about the evolution of the field. You can subscribe to the [Atom feed for this blog](http://blog.computer-networking.info/feed.xml). These notes are also posted on [the ebook's Facebook page](https://www.facebook.com/Computer-Networking-Principles-Protocols-and-Practice-129951043755620/)*
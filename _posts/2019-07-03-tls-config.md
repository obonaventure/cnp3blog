---
layout: post
title: A simpler way to correctly configure TLS servers
tag: tls, configuration
author: Olivier Bonaventure
---

The [Transport Layer Security (TLS)](https://www.computer-networking.info/2nd/html/protocols/tls.html) protocol plays a more and more important role in today's Internet. It secures wbesites, but also mail servers and a wide range of other services. 
Like many security protocols, TLS can be configured in very different ways and a minor change in a configuration file might have an important impact on the security of a TLS deployment. 
Another factor that contributes to the complexity of configuring TLS is that there are different implementations of this protocol that can be integrated in very different web servers. Each web server uses its own configuration file and different web servers use different parameters.

To simplify the installation of secure TLS webservers, [Mozilla](https://www.mozilla.com) has recently released a website that allows to automatically generate the configuration for OpenSSL and a dozen of different web servers and proxies. See [https://ssl-config.mozilla.org/](https://ssl-config.mozilla.org/)



*This blog post was written to inform the readers of [Computer Networking : Principles, Protocols and Practice](https://www.computer-networking.info) about the evolution of the field. You can subscribe to the Atom feed for this blog at [https://obonaventure.github.io/cnp3blog/feed.xml](https://obonaventure.github.io/cnp3blog/feed.xml). These notes are also posted on [the ebook's Facebook page](https://www.facebook.com/Computer-Networking-Principles-Protocols-and-Practice-129951043755620/)*

---
layout: post
title: Many Internet hosts often phone home
tag: ubuntu, privacy
author: Olivier Bonaventure
---

While collecting some DNS packet traces to prepare new DNS exercises for the students, I was surprised to notice that my Linux host was regularly doing DNS requests for [connectivity-check.ubuntu.com]

![Ubuntu Connectivity check]({{ site.baseurl }}/images/ubuntu-check.png)

It turns out that this is part of Ubuntu's automatic checks to verify that the Internet connection correctly works. It is also a way to detect captive Wi-Fi portals. It makes sense to run it on a laptop when it is connected to a Wi-Fi network, but I'm not convinced that a PC connected to the Ethernet network really needs this feature. Ubuntu is not the only type of operating system that regularly phones home, Microsoft and Apple also user similar techniques.

I looked a bit more at the DNS requests that my Ubuntu PC sends. It turns out that connectivity-check.ubuntu.com is only reachable over IPv4. There is no AAAA record that corresponds to this domain name. This implies that if you are connected to a network that only supports IPv6, the connectivity check will not work.

It is interesting to look at [http://connectivity-check.ubuntu.com](http://connectivity-check.ubuntu.com) and
[https://connectivity-check.ubuntu.com](https://connectivity-check.ubuntu.com).

```
curl --verbose http://connectivity-check.ubuntu.com
* Rebuilt URL to: http://connectivity-check.ubuntu.com/
*   Trying 35.222.85.5...
* TCP_NODELAY set
* Connected to connectivity-check.ubuntu.com (35.222.85.5) port 80 (#0)
> GET / HTTP/1.1
> Host: connectivity-check.ubuntu.com
> User-Agent: curl/7.58.0
> Accept: */*
> 
< HTTP/1.1 204 No Content
< Date: Tue, 03 Sep 2019 20:24:59 GMT
< Server: Apache/2.4.18 (Ubuntu)
< X-NetworkManager-Status: online
```

Over http, the server simply replies that the client is online. The same response is returned over https.

```
curl --verbose https://connectivity-check.ubuntu.com
* Rebuilt URL to: https://connectivity-check.ubuntu.com/
*   Trying 35.222.85.5...
* TCP_NODELAY set
* Connected to connectivity-check.ubuntu.com (35.222.85.5) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: /etc/ssl/certs
* (304) (OUT), TLS handshake, Client hello (1):
* (304) (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Client hello (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES128-GCM-SHA256
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: CN=connectivity-check.ubuntu.com
*  start date: Jul 28 11:08:08 2019 GMT
*  expire date: Oct 26 11:08:08 2019 GMT
*  subjectAltName: host "connectivity-check.ubuntu.com" matched cert's "connectivity-check.ubuntu.com"
*  issuer: C=US; O=Let's Encrypt; CN=Let's Encrypt Authority X3
*  SSL certificate verify ok.
> GET / HTTP/1.1
> Host: connectivity-check.ubuntu.com
> User-Agent: curl/7.58.0
> Accept: */*
> 
< HTTP/1.1 204 No Content
< Date: Tue, 03 Sep 2019 20:25:17 GMT
< Server: Apache/2.4.18 (Ubuntu)
< X-NetworkManager-Status: online
```

A closer look at the packets exchanged by my Linux PC show that it uses http by default and thus sends the connectivity check in cleartext.

```
    192.168.0.36.47110 > 35.224.99.156.80: Flags [P.], cksum 0x48c6 (incorrect -> 0x4d97), seq 1:88, ack 1, win 229, options [nop,nop,TS val 2182641267 ecr 3107375186], length 87: HTTP, length: 87
	GET / HTTP/1.1
	Host: connectivity-check.ubuntu.com
	Accept: */*
	Connection: close
	
```

The good news about the utilisation of http is that we can easily verify that Ubuntu does not use this connectivity check to send information about the configuration of my PC. However, it also implies that any Wi-Fi access point could easily detect which attached hosts use Ubuntu...

If you use Ubuntu, you can [disable this frequent connectivity check](https://vitux.com/disable-connectivity-checking-on-ubuntu-for-public-wifi-captive-portals/). On my PC, this request was sent every 5 minutes... I wonder the number of requests that Ubuntu receives every day on their servers.

*This blog post was written to inform the readers of [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info) about the evolution of the field. You can subscribe to the [Atom feed for this blog](http://blog.computer-networking.info/feed.xml). These notes are also posted on [the ebook's Facebook page](https://www.facebook.com/Computer-Networking-Principles-Protocols-and-Practice-129951043755620/)*
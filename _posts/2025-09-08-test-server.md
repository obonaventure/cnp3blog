---
layout: post
title: Test services for networking students
tag: test
author: Olivier Bonaventure
---

When students start to learn how the Internet operates, it is important for them to be able to experiment with real servers and Internet applications. To encourage the readers of [Computer Networking: Principles, Protocols and Practice](https://www.computer-networking.info), we have started to install small basic services on a VPS.

The first service is the `echo` service defined in [RFC862](https://datatracker.ietf.org/doc/rfc862/). When a client connects to the server using TCP, the server echoes all the bytes sent by the client over the TCP connection.

Students can test this service using `telnet`, [netcat/nc](https://en.wikipedia.org/wiki/Netcat) or even `curl`.

```nc echo.computer-networking.info 7
Hello
Hello
^C
```
```
curl telnet://echo.computer-networking.info:7
Hello
Hello
```

RFC862 also defines this service for UDP, but blindly echoing UDP messages is risky from a security viewpoint.

We also support the Discard protocol defined in [RFC863](https://datatracker.ietf.org/doc/html/rfc863). This service simply accepts TCP connections or UDP messages on port 9 and simply discards them without any reply.

```nc discard.computer-networking.info 9
Hello
^C```

And for UDP:

```
nc -u discard.computer-networking.info 9
hello
another
```

The last service is the daytime protocol specified in [RFC867](https://datatracker.ietf.org/doc/html/rfc867). This service simply accepts TCP connections and returns the time of the day on the server. It uses port 13 by default. For security reasons, we disable the UDP service.

```
nc daytime.computer-networking.info 13
08 SEP 2025 13:44:50 UTC
```

These services will be used by examples in the fourth edition of the [Computer Networking: Principles, Protocols, and Practice](https://www.computer-networking.info) ebook. 

This blog aims at encouraging students who read the open [Computer Networking: Principles, Protocols, and Practice](https://www.computer-networking.info) ebook to explore new networking topics. You can follow this blog by subscribing to its [RSS feed](http://blog.computer-networking.info/feed.xml) or by following [@cnp3_ebook on Mastodon](https://mastodon.acm.org/@cnp3_ebook). Feel free to share the posts that you find interesting on your preferred social network.
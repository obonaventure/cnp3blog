---
layout: post
title: Exploring static routing with IPMininet
tag: IPv6, ICMPv6, static routing
author: Olivier Bonaventure
---

In a [previous post](https://obonaventure.github.io/cnp3blog/ipmininet) we have shown that [IPMininet](https://github.com/oliviertilmans/ipmininet)
can be used to develop exercises that enable students to explore how IPv6 routers forward
packets. We used a simple example with only three routers and very simple static routes. In this
post, we build a larger network and introduce different static routes on the main routers.
Our [IPMininet](https://github.com/oliviertilmans/ipmininet) network contains two hosts
and five routers.

```console
mininet> nodes
available nodes are: 
h1 h2 ra rb rc re
mininet> links
ra-eth2<->h1-eth0 (OK OK)
ra-eth0<->rb-eth0 (OK OK)
ra-eth1<->rc-eth0 (OK OK)
rb-eth1<->rc-eth1 (OK OK)
rb-eth2<->re-eth0 (OK OK)
rc-eth2<->re-eth1 (OK OK)
re-eth2<->h2-eth0 (OK OK)
```

From this output, it is easy to represent the network as a graph:

```console
       +------------+
       |            |
h1--- ra --- rb --- rc
             |       |
      h2 --- re------+
```

The two hosts are configured with the following IPv6 addresses and a default route
that points to their router.

```console
mininet> h2 ip -6 addr show dev h2-eth0 
2: h2-eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:2::2/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::4450:34ff:fe19:6c58/64 scope link 
       valid_lft forever preferred_lft forever
mininet> h1 ip -6 addr show dev h1-eth0 
2: h1-eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:1::1/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::5895:51ff:fee2:226b/64 scope link 
       valid_lft forever preferred_lft forever
```

```console
mininet> h1 ip -6 route show             
2001:2345:1::/64 dev h1-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev h1-eth0  proto kernel  metric 256  pref medium
default via 2001:2345:1::a dev h1-eth0  metric 1024  pref medium
mininet> h2 ip -6 route show 
2001:2345:2::/64 dev h2-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev h2-eth0  proto kernel  metric 256  pref medium
default via 2001:2345:2::e dev h2-eth0  metric 1024  pref medium
````

Our [first example](https://github.com/obonaventure/RoutingExamples/blob/master/static/static-example1.py) includes static routes that enable `h1` to reach `h2` correctly:

```console
mininet> h1 traceroute6 2001:2345:2::2
traceroute to 2001:2345:2::2 (2001:2345:2::2) from 2001:2345:1::1, 30 hops max, 16 byte packets
 1  2001:2345:1::a (2001:2345:1::a)  0.479 ms  0.047 ms  0.124 ms
 2  2001:2345:4::c (2001:2345:4::c)  0.162 ms  0.166 ms  0.101 ms
 3  2001:2345:3::e (2001:2345:3::e)  0.157 ms  0.184 ms  0.198 ms
 4  2001:2345:2::2 (2001:2345:2::2)  0.239 ms  0.097 ms  0 ms
mininet> h2 traceroute6 2001:2345:1::1
traceroute to 2001:2345:1::1 (2001:2345:1::1) from 2001:2345:2::2, 30 hops max, 16 byte packets
 1  2001:2345:2::e (2001:2345:2::e)  0.067 ms  0.048 ms  0.025 ms
 2  2001:2345:3::c (2001:2345:3::c)  0.037 ms  0.036 ms  0.023 ms
 3  2001:2345:4::a (2001:2345:4::a)  0.035 ms  0.035 ms  0.025 ms
 4  2001:2345:1::1 (2001:2345:1::1)  0.031 ms  0.073 ms  0.025 ms
```

This can be confirmed by looking at the IPv6 routing tables of routers `ra`, `rc` and `re`
(the loopback addresses have been removed from the output of the `ip` command ) :

```console
mininet> ra ip -6 addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 state UNKNOWN qlen 1
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ra-eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:1::a/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::5cee:dff:fe3a:bb4b/64 scope link 
       valid_lft forever preferred_lft forever
3: ra-eth0@ra-eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:7::a/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::acd5:1ff:fe69:9cd/64 scope link 
       valid_lft forever preferred_lft forever
4: ra-eth1@ra-eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:4::a/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::4497:7aff:fe86:86b7/64 scope link 
       valid_lft forever preferred_lft forever
mininet> rc ip -6 addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 state UNKNOWN qlen 1
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: rc-eth0@rc-eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:4::c/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::ec32:67ff:fe79:435/64 scope link 
       valid_lft forever preferred_lft forever
3: rc-eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:5::c/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::cca:a9ff:fe17:67e9/64 scope link 
       valid_lft forever preferred_lft forever
4: rc-eth2@rc-eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:3::c/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::8c3f:e6ff:fe98:f3e5/64 scope link 
       valid_lft forever preferred_lft forever
mininet> re ip -6 addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 state UNKNOWN qlen 1
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: re-eth0@re-eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:6::e/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::c87d:10ff:fe35:5b25/64 scope link 
       valid_lft forever preferred_lft forever
3: re-eth1@re-eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:3::e/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::cc07:59ff:fe97:3a7d/64 scope link 
       valid_lft forever preferred_lft forever
4: re-eth2@re-eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:2::e/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::bcfd:cff:fedf:fedb/64 scope link 
       valid_lft forever preferred_lft forever
```

```console
mininet> ra ip -6 route show
2001:2345:1::/64 dev ra-eth2  proto kernel  metric 256  pref medium
2001:2345:4::/64 dev ra-eth1  proto kernel  metric 256  pref medium
2001:2345:7::/64 dev ra-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev ra-eth2  proto kernel  metric 256  pref medium
fe80::/64 dev ra-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev ra-eth1  proto kernel  metric 256  pref medium
default via 2001:2345:4::c dev ra-eth1  proto zebra  metric 20  pref medium
mininet> rb ip -6 route show
2001:2345:5::/64 dev rb-eth1  proto kernel  metric 256  pref medium
2001:2345:6::/64 dev rb-eth2  proto kernel  metric 256  pref medium
2001:2345:7::/64 dev rb-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev rb-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev rb-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev rb-eth2  proto kernel  metric 256  pref medium
default via 2001:2345:5::c dev rb-eth1  proto zebra  metric 20  pref medium
mininet> rc ip -6 route show
2001:2345:1::/48 via 2001:2345:4::a dev rc-eth0  proto zebra  metric 20  pref medium
2001:2345:2::/48 via 2001:2345:3::e dev rc-eth2  proto zebra  metric 20  pref medium
2001:2345:3::/64 dev rc-eth2  proto kernel  metric 256  pref medium
2001:2345:4::/64 dev rc-eth0  proto kernel  metric 256  pref medium
2001:2345:5::/64 dev rc-eth1  proto kernel  metric 256  pref medium
2001:2345:6::/48 via 2001:2345:5::b dev rc-eth1  proto zebra  metric 20  pref medium
2001:2345:7::/48 via 2001:2345:5::b dev rc-eth1  proto zebra  metric 20  pref medium
fe80::/64 dev rc-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev rc-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev rc-eth2  proto kernel  metric 256  pref medium
mininet> re ip -6 route show
2001:2345:2::/64 dev re-eth2  proto kernel  metric 256  pref medium
2001:2345:3::/64 dev re-eth1  proto kernel  metric 256  pref medium
2001:2345:6::/64 dev re-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev re-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev re-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev re-eth2  proto kernel  metric 256  pref medium
default via 2001:2345:3::c dev re-eth1  proto zebra  metric 20  pref medium
```

From this information, can you predict the output that `traceroute6` will produce between
any pair of routers in the network ? You can check your result by using `traceroute6` from
any router or host towards any reachable IPv6 address inside the emulated network. Here is a simple
script to automate those traceroutes.

```console
#!/bin/bash

for dest in  2001:2345:1::1  2001:2345:2::2 2001:2345:1::a  2001:2345:7::a 2001:2345:4::a  2001:2345:7::b  2001:2345:5::b 2001:2345:6::b 2001:2345:4::c   2001:2345:5::c  2001:2345:3::c 2001:2345:6::e 2001:2345:3::e  2001:2345:2::e
do
    echo "Running traceroute6 towards ${dest} "
    traceroute6 -q 1 ${dest}
done
```

Another network scenario is provided in [static-err.py](https://github.com/obonaventure/RoutingExamples/blob/master/static/static-err.py). This scenario uses the
same network topology and IPv6 addresses, but the static routes are different.

```console
mininet> ra ip -6 route show
2001:2345:1::/64 dev ra-eth2  proto kernel  metric 256  pref medium
2001:2345:2::/47 via 2001:2345:7::b dev ra-eth0  proto zebra  metric 20  pref mdium
2001:2345:4::/64 dev ra-eth1  proto kernel  metric 256  pref medium
2001:2345:7::/64 dev ra-eth0  proto kernel  metric 256  pref medium
2001:2345:4::/46 via 2001:2345:4::c dev ra-eth1  proto zebra  metric 20  pref mdium
fe80::/64 dev ra-eth2  proto kernel  metric 256  pref medium
fe80::/64 dev ra-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev ra-eth1  proto kernel  metric 256  pref medium
mininet> rc ip -6 route show
2001:2345::/47 via 2001:2345:4::a dev rc-eth0  proto zebra  metric 20  pref medum
2001:2345:3::/64 dev rc-eth2  proto kernel  metric 256  pref medium
2001:2345:2::/47 via 2001:2345:3::e dev rc-eth2  proto zebra  metric 20  pref mdium
2001:2345:4::/64 dev rc-eth0  proto kernel  metric 256  pref medium
2001:2345:5::/64 dev rc-eth1  proto kernel  metric 256  pref medium
2001:2345:6::/47 via 2001:2345:5::b dev rc-eth1  proto zebra  metric 20  pref mdium
fe80::/64 dev rc-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev rc-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev rc-eth2  proto kernel  metric 256  pref medium
mininet> re ip -6 route show
2001:2345:2::/64 dev re-eth2  proto kernel  metric 256  pref medium
2001:2345:3::/64 dev re-eth1  proto kernel  metric 256  pref medium
2001:2345:5::/48 via 2001:2345:3::c dev re-eth1  proto zebra  metric 20  pref mdium
2001:2345:6::/64 dev re-eth0  proto kernel  metric 256  pref medium
2001:2345::/40 via 2001:2345:6::b dev re-eth0  proto zebra  metric 20  pref medum
fe80::/64 dev re-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev re-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev re-eth2  proto kernel  metric 256  pref medium
mininet> rb ip -6 route show
2001:2345:3::/48 via 2001:2345:5::c dev rb-eth1  proto zebra  metric 20  pref medium
2001:2345:2::/47 via 2001:2345:6::e dev rb-eth2  proto zebra  metric 20  pref medium
2001:2345:5::/64 dev rb-eth1  proto kernel  metric 256  pref medium
2001:2345:6::/64 dev rb-eth2  proto kernel  metric 256  pref medium
2001:2345:7::/64 dev rb-eth0  proto kernel  metric 256  pref medium
2001:2345:4::/46 via 2001:2345:5::c dev rb-eth1  proto zebra  metric 20  pref medium
2001:2345::/44 via 2001:2345:7::a dev rb-eth0  proto zebra  metric 20  pref medium
fe80::/64 dev rb-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev rb-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev rb-eth2  proto kernel  metric 256  pref medium
```

In these routing tables, note the length of the different prefixes. When analysing how
packets are forwarded by each router, remember that they use the most specific match in the
routing table.




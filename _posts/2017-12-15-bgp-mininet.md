---
layout: post
title: Exploring BGP with IPMininet
tag: BGP, IPv6, IPMininet
author: Olivier Bonaventure
---


[IPMininet](https://github.com/oliviertilmans/ipmininet) supports various routing protocols. In this post,
we use it to study how the Border Gateway Protocol operates in a simple network containing only
BGP routers. Our virtual lab contains four routers and four hosts:


```console

                     h2
                     ||
       h1 = ra ----- rb ----- rd = h4
            |        |
            +------ rc = h3
```

The [IPMininet](https://github.com/oliviertilmans/ipmininet) script for this lab is available
from [https://github.com/obonaventure/RoutingExamples/blob/master/bgp/simple-bgp.py](https://github.com/obonaventure/RoutingExamples/blob/master/bgp/simple-bgp.py). There are four main parts in the definition of this topology.

First, we create the BGP routers and specify the prefixes that each router advertises.

```python

        # BGP routers

        as1ra = self.bgp('as1ra',['2001:1234:1::/64'])
        as2rb = self.bgp('as2rb',['2001:1234:2::/64'])
        as3rc = self.bgp('as3rc',['2001:1234:3::/64'])
        as4rd = self.bgp('as4rd',['2001:1234:4::/64'])
```

The first router, `as1ra` will use BGP to advertise prefix `2001:1234:1::/64` that is
assigned to the LAN containing host `h1`. Then, we associate each router to an
Autonomous System.

```python
       # Set AS-ownerships

        self.addOverlay(AS(1, (as1ra,)))
        self.addOverlay(AS(2, (as2rb,)))
        self.addOverlay(AS(3, (as3rc,)))
        self.addOverlay(AS(4, (as4rd,)))
```

This information is used by [IPMininet](https://github.com/oliviertilmans/ipmininet) to
create the BGP configuration for the [Quagga dameon](http://www.nongnu.org/quagga/docs/quagga.html).
We can then create the links between the different routers. We configure each interdomain link with
its own IPv6 prefix. Kato and Manning have proposed to use [link-local addresses for the eBGP sessions](https://tools.ietf.org/html/draft-kato-bgp-ipv6-link-local-00) and several vendors have implemented this feature, but we do not analyse it here.

```python 

 # Inter-AS links

        self.addLink(as1ra, as2rb,                      
                     params1={"ip": "2001:12::a/64"},
                     params2={"ip": "2001:12::b/64"})
        self.addLink(as1ra, as3rc,                      
                     params1={"ip": "2001:13::a/64"},
                     params2={"ip": "2001:13::c/64"})
        self.addLink(as2rb, as3rc,                      
                     params1={"ip": "2001:23::b/64"},
                     params2={"ip": "2001:23::c/64"})
        self.addLink(as2rb, as4rd,                      
                     params1={"ip": "2001:24::c/64"},
                     params2={"ip": "2001:24::d/64"})

```

The topology and the IPv6 addresses have been allocated. We can now create the peering links.

```python

        # Add eBGP peering
        bgp_peering(self, as1ra, as2rb)
        bgp_peering(self, as1ra, as3rc)
        bgp_peering(self, as2rb, as3rc)
        bgp_peering(self, as2rb, as4rd)

```

These methods create the configuration files for the Quagga BGP daemon based on the parameters that we
have passed. For example, let us look at the BGP configuration file for `as1ra`.

```console
hostname as1ra
password zebra

log file /tmp/bgpd_as1ra.log


router bgp 1
    bgp router-id 0.0.0.2
    bgp bestpath compare-routerid
    no auto-summary
    neighbor 2001:12::b remote-as 2
    neighbor 2001:12::b port 179
    neighbor 2001:12::b description as2rb (eBGP)
    neighbor 2001:12::b ebgp-multihop
    
    no auto-summary
    neighbor 2001:13::c remote-as 3
    neighbor 2001:13::c port 179
    neighbor 2001:13::c description as3rc (eBGP)
    neighbor 2001:13::c ebgp-multihop
    
    address-family ipv6
    network 2001:1234:1::/64
    neighbor 2001:12::b activate
    neighbor 2001:12::b next-hop-self all
    neighbor 2001:13::c activate
    neighbor 2001:13::c next-hop-self all
    exit-address-family
    !

!
```

After having defined the `hostname`, default password and log file, we start the BGP part of the configuration
by indicating the AS number of `as1ra` with `router bgp 1`. The next line specifies the `router-id` of the 
BGP router. Each BGP router must have a unique router-id which is usually one of its IPv4 addresses.
In our examples, we do not assign IPv4 addresses to the routers and  [IPMininet](https://github.com/oliviertilmans/ipmininet)
automatically assigns router-id `0.0.0.2` to `as1ra`. The lines starting with the `neighbor` commands define the
eBGP sessions between `as1ra` and `as2rb` and `as3rc`. Each session is an external BGP session and uses port
179 which is the standard BGP port. The last part of the configuration specifies that router `as1ra` will advertise
the `2001:1234:1::/64` IPv6 prefix.

We can now start the network with `sudo python bgp/simple-bgp.py`. Let us first check that all hosts can reach the others.
The latest version of [IPMininet](https://github.com/oliviertilmans/ipmininet) includes an updated version of the
`pingall` command which supports IPv4 and IPv6.

```console

mininet>pingall
*** Ping: testing reachability over IPv4 and IPv6
h1 --IPv6--> h4 h3 h2 
h2 --IPv6--> h4 h1 h3 
h3 --IPv6--> h4 h1 h2 
h4 --IPv6--> h1 h2 h3 
*** Results: 0% dropped (12/12 received)
```

This confirms that the routing tables are correct. We can verify some of the paths by using `traceroute6`.

```console
mininet> h1 traceroute6 h2
traceroute to 2001:1234:2::2 (2001:1234:2::2) from 2001:1234:1::1, 30 hops max, 16 byte packets
 1  2001:1234:1::a (2001:1234:1::a)  0.088 ms  0.131 ms  0.063 ms
 2  2001:12::b (2001:12::b)  0.056 ms  0.045 ms  0.035 ms
 3  2001:1234:2::2 (2001:1234:2::2)  0.096 ms  0.039 ms  0.106 ms
mininet> h4 traceroute6 h1
traceroute to 2001:1234:1::1 (2001:1234:1::1) from 2001:1234:4::4, 30 hops max, 16 byte packets
 1  2001:1234:4::d (2001:1234:4::d)  0.059 ms  0.057 ms  0.025 ms
 2  2001:24::c (2001:24::c)  0.037 ms  0.131 ms  0.045 ms
 3  2001:12::a (2001:12::a)  0.053 ms  0.046 ms  0.043 ms
 4  2001:1234:1::1 (2001:1234:1::1)  0.043 ms  0.087 ms  0.048 ms
```

The BGP daemon which runs on each router provides a command line interface (CLI) which can be used
to issue commands but also to check the status of the routing protocols. We can start
this CLI with `telnet ::1 bgpd`. In this command, `bgpd` is translated into port number 2605 which is 
defined in the `/etc/services` file on `as1ra`.


```console
mininet> as1ra telnet ::1 bgpd
Trying ::1...
Connected to ::1.
Escape character is '^]'.

Hello, this is Quagga (version 1.2.2).
Copyright 1996-2005 Kunihiro Ishiguro, et al.


User Access Verification

Password: zebra

as1ra> 
```

A first command is `show bgp summary` which outputs a summary of the status of the BGP
sessions on the local router.

```console
as1ra> sh bgp summary 

...

IPv6 Unicast Summary:
---------------------
BGP router identifier 0.0.0.2, local AS number 1
RIB entries 7, using 504 bytes of memory
Peers 2, using 9888 bytes of memory

Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2001:12::b      4     2      31      33        0    0    0 00:25:26        3
2001:13::c      4     3      31      34        0    0    0 00:25:26        3

Total number of neighbors 2

Total num. Established sessions 2
Total num. of routes received     6

...
```

The output is verbose. We only list above the information related to the IPv6 routes
that we use. The two eBGP sessions that we have defined in the configuration are
active and 3 prefixes have been learned from each of these sessions.

We can obtain more information about the BGP sessions with the `show bgp neighbors` command.

```console

as1ra> show bgp neighbors 2001:13::c
BGP neighbor is 2001:13::c, remote AS 3, local AS 1, external link
 Description: as3rc (eBGP)
  BGP version 4, remote router ID 0.0.0.4
  BGP state = Established, up for 00:46:29
  Last read 00:00:29, hold time is 180, keepalive interval is 60 seconds
  Neighbor capabilities:
    4 Byte AS: advertised and received
    Route refresh: advertised and received(old & new)
    Address family IPv4 Unicast: advertised and received
    Address family IPv6 Unicast: advertised and received
    Graceful Restart Capabilty: advertised and received
      Remote Restart timer is 120 seconds
      Address families by peer:
        none
  Graceful restart informations:
    End-of-RIB send: IPv4 Unicast, IPv6 Unicast
    End-of-RIB received: IPv4 Unicast
  Message statistics:
    Inq depth is 0
    Outq depth is 0
                         Sent       Rcvd
    Opens:                  2          0
    Notifications:          0          0
    Updates:                5          5
    Keepalives:            48         47
    Route Refresh:          0          0
    Capability:             0          0
    Total:                 55         52
  Minimum time between advertisement runs is 3 seconds

 For address family: IPv4 Unicast
  Community attribute sent to this neighbor(all)
  0 accepted prefixes

 For address family: IPv6 Unicast
  NEXT_HOP is always this router
  Community attribute sent to this neighbor(all)
  3 accepted prefixes

  Connections established 1; dropped 0
  Last reset never
  External BGP neighbor may be up to 255 hops away.
Local host: 2001:13::a, Local port: 179
Foreign host: 2001:13::c, Foreign port: 46614
Nexthop: 0.0.0.2
Nexthop global: 2001:13::a
Nexthop local: fe80::9442:91ff:fe35:b5b1
BGP connection: shared network
Read thread: on  Write thread: off
```


Another very important command is `show ipv6 bgp`. It outputs the BGP routing
table of the router.

```console
as1ra> show ipv6 bgp
BGP table version is 0, local router ID is 0.0.0.2
Status codes: s suppressed, d damped, h history, * valid, > best, = multipath,
              i internal, r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*> 2001:1234:1::/64 ::                       0         32768 i
*  2001:1234:2::/64 2001:13::c                             0 3 2 i
*>                  2001:12::b               0             0 2 i
*  2001:1234:3::/64 2001:12::b                             0 2 3 i
*>                  2001:13::c               0             0 3 i
*  2001:1234:4::/64 2001:13::c                             0 3 2 4 i
*>                  2001:12::b                             0 2 4 i

Displayed  4 out of 7 total prefixes
```

It is important to understand the information in the BGP routing table. The first prefix,
`2001:1234:1::/64` is the one which is originated by `as1ra`. Its origin is
marked as being interal (see character `i` at the end of the line). The first prefix
that we learn from BGP is `2001:1234:2::/64`. `as1ra` has received two paths
for this prefix : `AS3:AS2` via router `as3rc` (nexthop `2001:13::c`) and
`AS2` via router `as2rb` (nexthop `2001:12::b` ). 
According to the [BGP Decision Process](http://cnp3book.info.ucl.ac.be/2nd/html/protocols/bgp.html) , the path via `as2rb` is preferred since it has
the shortes AS-Path. The `*` character at the beginning of each line indicates that the paths
are valid and the `>` character indicates the best path which is installed in the FIB
and used to forwarded data plane packets. Prefix `2001:1234:3::/64` is learned from
both `as2rb` and `as3rc` and the latter path is preferred. Prefix `2001:1234:4::/64` is
learned from both `as2rb` and `as3rc` and in this case the former is preffered.

Once the BGP sessions are up, it is interesting to observe the BGP messages that are exchanged. 
Routers only exchange BGP routes when they changed. In our stable topology, we only observe BGP Update
messages when sessions are created or stopped. The most frequent BGP messages that we can
observe are the BGP Keepalive messages that are exchanged regularly between routers to
check that the BGP sessions remain up. As an example, here are the BGP Update messages
observed on the BGP session between `as1ra` and `as2rb`.

```console
14:14:40.122437 IP6 (class 0xc0, flowlabel 0x147fb, hlim 255, next-header TCP (6) payload length: 51) 2001:12::a.179 > 2001:12::b.54230: Flags [P.], cksum 0x4074 (incorrect -> 0x6e6e), seq 2508:2527, ack 2509, win 60, options [nop,nop,TS val 7219330 ecr 7204330], length 19: BGP
        Keepalive Message (4), length: 19
14:14:40.122637 IP6 (class 0xc0, flowlabel 0x67094, hlim 255, next-header TCP (6) payload length: 51) 2001:12::b.54230 > 2001:12::a.179: Flags [P.], cksum 0x4074 (incorrect -> 0x33c4), seq 2509:2528, ack 2527, win 59, options [nop,nop,TS val 7219330 ecr 7219330], length 19: BGP
        Keepalive Message (4), length: 19
14:14:40.122647 IP6 (class 0xc0, flowlabel 0x147fb, hlim 255, next-header TCP (6) payload length: 32) 2001:12::a.179 > 2001:12::b.54230: Flags [.], cksum 0x4061 (incorrect -> 0x37de), seq 2527, ack 2528, win 60, options [nop,nop,TS val 7219330 ecr 7219330], length 0
14:15:40.122793 IP6 (class 0xc0, flowlabel 0x67094, hlim 255, next-header TCP (6) payload length: 51) 2001:12::b.54230 > 2001:12::a.179: Flags [P.], cksum 0x4074 (incorrect -> 0xf918), seq 2528:2547, ack 2527, win 59, options [nop,nop,TS val 7234330 ecr 7219330], length 19: BGP
        Keepalive Message (4), length: 19
14:15:40.122827 IP6 (class 0xc0, flowlabel 0x147fb, hlim 255, next-header TCP (6) payload length: 32) 2001:12::a.179 > 2001:12::b.54230: Flags [.], cksum 0x4061 (incorrect -> 0xc29a), seq 2527, ack 2547, win 60, options [nop,nop,TS val 7234330 ecr 7234330], length 0
14:15:40.123176 IP6 (class 0xc0, flowlabel 0x147fb, hlim 255, next-header TCP (6) payload length: 51) 2001:12::a.179 > 2001:12::b.54230: Flags [P.], cksum 0x4074 (incorrect -> 0xbe6c), seq 2527:2546, ack 2547, win 60, options [nop,nop,TS val 7234330 ecr 7234330], length 19: BGP
```

Looking at the timing between the messages, we can easily observe that BGP Keepalive messages are
exchanged every minute. This is the default timer period with our version of Quagga.

Let us now explore what happens when we disable the link between `as1ra` and `as2rb`. For this, we 
execute `ip link set dev as2rb-eth0 down` on router `as2rb` and observe the impact of
this command on router `as1ra`. After having issued this command the link between the
two routers appears to freeze as it does not anymore carry packets. If we run
`traceroute6` from host `h1`, we see clearly that the packets have been rerouted
via router `as3rc`.

```console
mininet> h1 traceroute6 h4
traceroute to 2001:1234:4::4 (2001:1234:4::4) from 2001:1234:1::1, 30 hops max, 16 byte packets
 1  2001:1234:1::a (2001:1234:1::a)  0.056 ms  0.04 ms  0.016 ms
 2  2001:13::c (2001:13::c)  0.025 ms  0.025 ms  0.022 ms
 3  2001:23::b (2001:23::b)  0.023 ms  0.095 ms  0.085 ms
 4  2001:24::d (2001:24::d)  0.051 ms  0.033 ms  0.019 ms
 5  2001:1234:4::4 (2001:1234:4::4)  0.025 ms  0.028 ms  0.017 ms
```

To understand the reason for this change of route, let us look at the content of
the BGP routing table on `as1ra`:

```console
as1ra> sh ipv6 bgp
BGP table version is 0, local router ID is 0.0.0.2
Status codes: s suppressed, d damped, h history, * valid, > best, = multipath,
              i internal, r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*> 2001:1234:1::/64 ::                       0         32768 i
*> 2001:1234:2::/64 2001:13::c                             0 3 2 i
                    2001:12::b               0             0 2 i
   2001:1234:3::/64 2001:12::b                             0 2 3 i
*>                  2001:13::c               0             0 3 i
*> 2001:1234:4::/64 2001:13::c                             0 3 2 4 i
                    2001:12::b                             0 2 4 i

Displayed  4 out of 7 total prefixes
```

The routes that were learned form `as2rb` are still part of the BGP routing table, but
they are now marked as invalid (the corresponding line does not contain the 
`*` character anymore. BGP considers these routes to be invalid because `as1ra` does
not know how to reach the corresponding BGP nexthop (`2001:12::b`). If we look at the
FIB on router `as1ra`, we clearly observe that the router does not have a route
to reach prefis `2001:12::b/64`.

```console
mininet> as1ra ip -6 route
2001:12::/64 dev as1ra-eth0  proto kernel  metric 256 linkdown  pref medium
2001:13::/64 dev as1ra-eth1  proto kernel  metric 256  pref medium
2001:1234:1::/64 dev as1ra-eth2  proto kernel  metric 256  pref medium
2001:1234:2::/64 via fe80::e827:73ff:fe2d:d497 dev as1ra-eth1  proto zebra  metric 20  pref medium
2001:1234:3::/64 via fe80::e827:73ff:fe2d:d497 dev as1ra-eth1  proto zebra  metric 20  pref medium
2001:1234:4::/64 via fe80::e827:73ff:fe2d:d497 dev as1ra-eth1  proto zebra  metric 20  pref medium
fe80::/64 dev as1ra-eth0  proto kernel  metric 256 linkdown  pref medium
fe80::/64 dev as1ra-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev as1ra-eth2  proto kernel  metric 256  pref medium
```

If we look at the packets on the `as1ra`-`as2rb` link, we observe the last packets
excahnged on this link:

```console
 
14:19:40.125910 IP6 (class 0xc0, flowlabel 0x67094, hlim 255, next-header TCP (6) payload length: 51) 2001:12::b.54230 > 2001:12::a.179: Flags [P.], cksum 0x4074 (incorrect -> 0xe912), seq 2604:2623, ack 2622, win 59, options [nop,nop,TS val 7294331 ecr 7294330], length 19: BGP
        Keepalive Message (4), length: 19
14:19:40.166495 IP6 (class 0xc0, flowlabel 0x147fb, hlim 255, next-header TCP (6) payload length: 32) 2001:12::a.179 > 2001:12::b.54230: Flags [.], cksum 0x4061 (incorrect -> 0xed21), seq 2622, ack 2623, win 60, options [nop,nop,TS val 7294341 ecr 7294331], length 0

```

A close look at the logs of `as1ra1` shows that the BGP session between `as1ra` and `as2rb` was considered to
be down 3 minutes after the link became down. This is by default BGP sends BGP Keepalive messages every
60 seconds and considers the session to be down when it has not received any response to three successive
BGP Keepalive messages.

```console
mininet> as1ra tail /tmp/bgpd_as1ra.log 
2017/12/15 14:22:40 BGP: %NOTIFICATION: sent to neighbor 2001:12::b 4/0 (Hold Timer Expired) 0 bytes 
2017/12/15 14:22:40 BGP: Notification sent to neighbor 2001:12::b:0: type 4/0
2017/12/15 14:22:40 BGP: %ADJCHANGE: neighbor 2001:12::b Down BGP Notification send
2017/12/15 14:28:00 BGP: Vty connection from ::1
```

This is confirmed by the BGP summary that shows that router `as1ra` is trying to reestablish
the session with `as2rb`.

```console
as1ra> sh bgp summary 

IPv6 Unicast Summary:
---------------------
BGP router identifier 0.0.0.2, local AS number 1
RIB entries 7, using 504 bytes of memory
Peers 2, using 9888 bytes of memory

Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2001:12::b      4     2     193     200        0    0    0 00:08:35 Connect    
2001:13::c      4     3     204     208        0    0    0 03:18:35        3
```

At this point, we can re-enable the link between the two routers with
`ip link set dev as2rb-eth0 up` and reconfigure the static address
on `as2rb` with `ip -6 addr add 2001:12::b/64 dev as2rb-eth0`. Shortly after, the
two routers attempt again to restablish the session between themselves.

A BGP session is always established over a TCP connection. We thus observe first
the three-way handshake that establishes the TCP connection on port 179:

```console
14:45:28.402708 IP6 (class 0xc0, flowlabel 0x2900f, hlim 255, next-header TCP (6) payload length: 40) 2001:12::a.38394 > 2001:12::b.179: Flags [S], cksum 0x4069 (incorrect -> 0xb993), seq 3505799218, win 28800, options [mss 1440,sackOK,TS val 7681400 ecr 0,nop,wscale 9], length 0
14:45:28.402766 IP6 (class 0xc0, flowlabel 0x43cfb, hlim 255, next-header TCP (6) payload length: 40) 2001:12::b.179 > 2001:12::a.38394: Flags [S.], cksum 0x4069 (incorrect -> 0xa74c), seq 1701541837, ack 3505799219, win 28560, options [mss 1440,sackOK,TS val 7681400 ecr 7681400,nop,wscale 9], length 0
14:45:28.402780 IP6 (class 0xc0, flowlabel 0x2900f, hlim 255, next-header TCP (6) payload length: 32) 2001:12::a.38394 > 2001:12::b.179: Flags [.], cksum 0x4061 (incorrect -> 0x455e), seq 1, ack 1, win 57, options [nop,nop,TS val 7681400 ecr 7681400], length 0
14:45:28.402951 IP6 (class 0xc0, flowlabel 0x2900f, hlim 255, next-header TCP (6) payload length: 99) 2001:12::a.38394 > 2001:12::b.179: Flags [P.], cksum 0x40a4 (incorrect -> 0x7603), seq 1:68, ack 1, win 57, options [nop,nop,TS val 7681400 ecr 7681400], length 67: BGP
        Open Message (1), length: 67
          Version 4, my AS 1, Holdtime 180s, ID 0.0.0.2
          Optional parameters, length: 38
            Option Capabilities Advertisement (2), length: 6
              Multiprotocol Extensions (1), length: 4
                AFI IPv4 (1), SAFI Unicast (1)
                0x0000:  0001 0001
            Option Capabilities Advertisement (2), length: 6
              Multiprotocol Extensions (1), length: 4
                AFI IPv6 (2), SAFI Unicast (1)
                0x0000:  0002 0001
            Option Capabilities Advertisement (2), length: 2
              Route Refresh (Cisco) (128), length: 0
            Option Capabilities Advertisement (2), length: 2
              Route Refresh (2), length: 0
            Option Capabilities Advertisement (2), length: 6
              32-Bit AS Number (65), length: 4
                 4 Byte AS 1
                0x0000:  0000 0001
            Option Capabilities Advertisement (2), length: 4
              Graceful Restart (64), length: 2
                Restart Flags: [none], Restart Time 120s
                0x0000:  0078
 ```

The first BGP message that we observe is the `Open` message which provides basic information about
the session and allows to advertise that are supported by the initiating router
(in this case, `as1ra`). The router provides the BGP version (4), its AS number (1), the
timer to consider a session to be down (180s) and its router id. It also advertises
optional capabilities, including the ability to support IPv6.

`as2rb` replies with a BGP message that contains its AS number (2), hold timer, router-id
and optional capabilities. This message also contains a keepalive.

```console
14:45:28.403426 IP6 (class 0xc0, flowlabel 0x43cfb, hlim 255, next-header TCP (6) payload length: 118) 2001:12::b.179 > 2001:12::a.38394: Flags [P.], cksum 0x40b7 (incorrect -> 0x61a8), seq 1:87, ack 68, win 56, options [nop,nop,TS val 7681400 ecr 7681400], length 86: BGP
        Open Message (1), length: 67
          Version 4, my AS 2, Holdtime 180s, ID 0.0.0.3
          Optional parameters, length: 38
            Option Capabilities Advertisement (2), length: 6
              Multiprotocol Extensions (1), length: 4
                AFI IPv4 (1), SAFI Unicast (1)
                0x0000:  0001 0001
            Option Capabilities Advertisement (2), length: 6
              Multiprotocol Extensions (1), length: 4
                AFI IPv6 (2), SAFI Unicast (1)
                0x0000:  0002 0001
            Option Capabilities Advertisement (2), length: 2
              Route Refresh (Cisco) (128), length: 0
            Option Capabilities Advertisement (2), length: 2
              Route Refresh (2), length: 0
            Option Capabilities Advertisement (2), length: 6
              32-Bit AS Number (65), length: 4
                 4 Byte AS 2
                0x0000:  0000 0002
            Option Capabilities Advertisement (2), length: 4
              Graceful Restart (64), length: 2
                Restart Flags: [none], Restart Time 120s
                0x0000:  0078
        Keepalive Message (4), length: 19
```

We can then observe the BGP Update messages that advertise routes. `as1ra` is the first
to send its update:


```console
14:45:29.406999 IP6 (class 0xc0, flowlabel 0x2900f, hlim 255, next-header TCP (6) payload length: 463) 2001:12::a.38394 > 2001:12::b.179: Flags [P.], cksum 0x4210 (incorrect -> 0xe1af), seq 106:537, ack 106, win 57, options [nop,nop,TS val 7681651 ecr 7681400], length 431: BGP
        Update Message (2), length: 23
          End-of-Rib Marker (empty NLRI)
        Update Message (2), length: 94
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::a, fe80::20ca:6fff:fe8a:98a4, nh-length: 32, no SNPA
              2001:1234:1::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000a fe80 0000 0000 0000 20ca 6fff
            0x0020:  fe8a 98a4 0040 2001 1234 0001 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 6, Flags [TE]: 1 
            0x0000:  0201 0000 0001
          Multi Exit Discriminator (4), length: 4, Flags [O]: 0
            0x0000:  0000 0000
        Update Message (2), length: 95
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::a, fe80::20ca:6fff:fe8a:98a4, nh-length: 32, no SNPA
              2001:1234:2::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000a fe80 0000 0000 0000 20ca 6fff
            0x0020:  fe8a 98a4 0040 2001 1234 0002 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 14, Flags [TE]: 1 3 2 
            0x0000:  0203 0000 0001 0000 0003 0000 0002
        Update Message (2), length: 91
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::a, fe80::20ca:6fff:fe8a:98a4, nh-length: 32, no SNPA
              2001:1234:3::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000a fe80 0000 0000 0000 20ca 6fff
            0x0020:  fe8a 98a4 0040 2001 1234 0003 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 10, Flags [TE]: 1 3 
            0x0000:  0202 0000 0001 0000 0003
        Update Message (2), length: 99
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::a, fe80::20ca:6fff:fe8a:98a4, nh-length: 32, no SNPA
              2001:1234:4::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000a fe80 0000 0000 0000 20ca 6fff
            0x0020:  fe8a 98a4 0040 2001 1234 0004 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 18, Flags [TE]: 1 3 2 4 
            0x0000:  0204 0000 0001 0000 0003 0000 0002 0000
            0x0010:  0004
        Update Message (2), length: 29
          Multi-Protocol Unreach NLRI (15), length: 3, Flags [O]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
              End-of-Rib Marker (empty NLRI)
            0x0000:  0002 01
```

For efficiency reasons, BGP packs multiple update messages in the same TCP segment. Here, we can obser the
different prefixes, their nexthops and the associated AS Path. Quagga advertises both the IPv6 address and the link-local address for each nexthop. `as2rb` sends its BGP Update shortly after:

```console

14:45:29.407455 IP6 (class 0xc0, flowlabel 0x43cfb, hlim 255, next-header TCP (6) payload length: 455) 2001:12::b.179 > 2001:12::a.38394: Flags [P.], cksum 0x4208 (incorrect -> 0xeab5), seq 106:529, ack 537, win 58, options [nop,nop,TS val 7681651 ecr 7681651], length 423: BGP
        Update Message (2), length: 23
          End-of-Rib Marker (empty NLRI)
        Update Message (2), length: 95
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::b, fe80::5c58:b0ff:fe0d:ef98, nh-length: 32, no SNPA
              2001:1234:1::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000b fe80 0000 0000 0000 5c58 b0ff
            0x0020:  fe0d ef98 0040 2001 1234 0001 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 14, Flags [TE]: 2 3 1 
            0x0000:  0203 0000 0002 0000 0003 0000 0001
        Update Message (2), length: 94
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::b, fe80::5c58:b0ff:fe0d:ef98, nh-length: 32, no SNPA
              2001:1234:2::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000b fe80 0000 0000 0000 5c58 b0ff
            0x0020:  fe0d ef98 0040 2001 1234 0002 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 6, Flags [TE]: 2 
            0x0000:  0201 0000 0002
          Multi Exit Discriminator (4), length: 4, Flags [O]: 0
            0x0000:  0000 0000
        Update Message (2), length: 91
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::b, fe80::5c58:b0ff:fe0d:ef98, nh-length: 32, no SNPA
              2001:1234:3::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000b fe80 0000 0000 0000 5c58 b0ff
            0x0020:  fe0d ef98 0040 2001 1234 0003 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 10, Flags [TE]: 2 3 
            0x0000:  0202 0000 0002 0000 0003
        Update Message (2), length: 91
          Multi-Protocol Reach NLRI (14), length: 46, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
            nexthop: 2001:12::b, fe80::5c58:b0ff:fe0d:ef98, nh-length: 32, no SNPA
              2001:1234:4::/64
            0x0000:  0002 0120 2001 0012 0000 0000 0000 0000
            0x0010:  0000 000b fe80 0000 0000 0000 5c58 b0ff
            0x0020:  fe0d ef98 0040 2001 1234 0004 0000
          Origin (1), length: 1, Flags [T]: IGP
            0x0000:  00
          AS Path (2), length: 10, Flags [TE]: 2 4 
            0x0000:  0202 0000 0002 0000 0004
        Update Message (2), length: 29
          Multi-Protocol Unreach NLRI (15), length: 3, Flags [O]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
              End-of-Rib Marker (empty NLRI)
            0x0000:  0002 01
```

Shortly after, we observe two additional messages where `as1ra` withdraws the routes for
`2001:1234:2::/64` and `2001:1234:4::/64` because its best path to reach this destination
is now the path via `as2rb`.

```console
14:45:29.458127 IP6 (class 0xc0, flowlabel 0x2900f, hlim 255, next-header TCP (6) payload length: 80) 2001:12::a.38394 > 2001:12::b.179: Flags [P.], cksum 0x4091 (incorrect -> 0x538c), seq 537:585, ack 529, win 59, options [nop,nop,TS val 7681664 ecr 7681651], length 48: BGP
        Update Message (2), length: 48
          Multi-Protocol Unreach NLRI (15), length: 21, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
              2001:1234:2::/64
              2001:1234:4::/64
            0x0000:  0002 0140 2001 1234 0002 0000 4020 0112
            0x0010:  3400 0400 00
```

We can confirm this by looking at the BGP routing table of `as1ra`:

```console
as1ra> sh bgp ipv6
BGP table version is 0, local router ID is 0.0.0.2
Status codes: s suppressed, d damped, h history, * valid, > best, = multipath,
              i internal, r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*> 2001:1234:1::/64 ::                       0         32768 i
*> 2001:1234:2::/64 2001:12::b               0             0 2 i
*                   2001:13::c                             0 3 2 i
*  2001:1234:3::/64 2001:12::b                             0 2 3 i
*>                  2001:13::c               0             0 3 i
*> 2001:1234:4::/64 2001:12::b                             0 2 4 i
*                   2001:13::c                             0 3 2 4 i

Displayed  4 out of 7 total prefixes
```

`as2rb` also sends a withdraw for prefix `2001:1234:1::/64` for similar reasons.

```console
14:45:29.458229 IP6 (class 0xc0, flowlabel 0x43cfb, hlim 255, next-header TCP (6) payload length: 71) 2001:12::b.179 > 2001:12::a.38394: Flags [P.], cksum 0x4088 (incorrect -> 0x98db), seq 529:568, ack 585, win 58, options [nop,nop,TS val 7681664 ecr 7681664], length 39: BGP
        Update Message (2), length: 39
          Multi-Protocol Unreach NLRI (15), length: 12, Flags [OE]: 
            AFI: IPv6 (2), SAFI: Unicast (1)
              2001:1234:1::/64
            0x0000:  0002 0140 2001 1234 0001 0000
```


At this point, the BGP session between the two routers is stable and only Keepalive messages are exchanged.

---
layout: post
title: Exploring OSPFv3 routing with IPMininet
tag: IPv6, OSPF
author: Olivier Bonaventure
---

In this post, we continue our exploration of using 
[IPMininet](https://github.com/oliviertilmans/ipmininet) to prepare
exercises that enable students to discover IPv6 routing. Our focus is
now on OSPFv3 defined in [RFC5340](https://tools.ietf.org/html/rfc5340). 
We consider a simple network that contains four routers and two hosts.

```console

       h1-- ra ==== rb ==== re -- h2
              |      |       |
              +----- rc -----+

```

The two hosts are configured with static IPv6 addresses:

```console
mininet> h1 ip -6 addr show dev h1-eth0
2: h1-eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:1::1/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::e45c:2eff:fec8:b0dc/64 scope link 
       valid_lft forever preferred_lft forever
mininet> h2 ip -6 addr show dev h2-eth0
2: h2-eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:2345:2::2/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::7858:f3ff:fe29:6558/64 scope link 
       valid_lft forever preferred_lft forever

```


In this network, all links have an IGP metric set to 1, except links `ra-rb` and
`rb-re` whose metrics are set to 5. This implies that packets sent by
`h1` towards `h2` should follow the path `ra-rc-re-h2`. `traceroute6` confirms that
this is indeed the case:

```console
mininet> h1 traceroute6 2001:2345:2::2
traceroute to 2001:2345:2::2 (2001:2345:2::2) from 2001:2345:1::1, 30 hops max, 16 byte packets
 1  2001:2345:1::a (2001:2345:1::a)  0.08 ms  0.057 ms  0.028 ms
 2  2001:2345:4::c (2001:2345:4::c)  0.048 ms  0.044 ms  0.031 ms
 3  2001:2345:3::e (2001:2345:3::e)  0.044 ms  0.048 ms  0.027 ms
 4  2001:2345:2::2 (2001:2345:2::2)  0.04 ms  0.045 ms  0.033 ms
```

A key feature of dynamic routing protocols such as OSPFv3 is that they
can react to link failures and reroute traffic shortly after the detection of the 
failure. Our network contains seven links.

```console
mininet> links
ra-eth2<->h1-eth0 (OK OK)
ra-eth0<->rb-eth0 (OK OK)
ra-eth1<->rc-eth0 (OK OK)
rb-eth1<->rc-eth1 (OK OK)
rb-eth2<->re-eth0 (OK OK)
rc-eth2<->re-eth1 (OK OK)
re-eth2<->h2-eth0 (OK OK)
```

To demonstrate the reaction of OSPFv3, we can disable the link between `ra` and `rc`with the following command : `ra ip link set dev ra-eth1 down`. Immediately after having
disabled this link, we lauchn a traceroute from `h1` to `h2`.


```console

mininet> h1 traceroute6 2001:2345:2::2
traceroute to 2001:2345:2::2 (2001:2345:2::2) from 2001:2345:1::1, 30 hops max, 16 byte packets
 1  2001:2345:1::a (2001:2345:1::a)  0.053 ms  0.029 ms  0.018 ms
 2  * 2001:2345:7::b (2001:2345:7::b)  0.146 ms  0.085 ms
 3  2001:2345:5::c (2001:2345:5::c)  0.086 ms  0.081 ms  0.083 ms
 4  2001:2345:3::e (2001:2345:3::e)  0.088 ms  0.088 ms  0.053 ms
 5  2001:2345:2::2 (2001:2345:2::2)  0.06 ms  0.058 ms  0.052 ms

```

This traceroute shows an interesting output. The `*` in the second line shows that
one of the packets with a HopLimit set to 2 did not trigger an ICMPv6 response. This
is likely because this packet was sent while the routers were updating their routing
table. The packet packet was forwarded correctly and passed through `rb`. The new
path is `h1->ra->rb->rc->re->h2`. We renable the `ra-rb` link with `ra ip link set dev ra-eth1 up `.

`traceroute6` shows the path followed by real packets inside the network. This
tool is frequently used by network administrators, but they can also rely on
additional information available on OSPFv3 routers. On each router, it is
possible to access the management interface via the `` telnet :: ospf6d`` command.
The default password is `zebra`. This `telnet` command provides an command-line
interface to the router which provides several commands.

```console
rb> help
Quagga VTY provides advanced help feature.  When you need help,
anytime at the command line please press '?'.

If nothing matches, the help list will be empty and you must backup
 until entering a '?' shows the available options.
Two styles of help are provided:
1. Full help is available when you are ready to enter a
command argument (e.g. 'show ?') and describes each possible
argument.
2. Partial help is provided when an abbreviated argument is entered
   and you want to know what arguments match the input
   (e.g. 'show me?'.)

rb> 
  echo      Echo a message back to the vty
  enable    Turn on privileged mode command
  exit      Exit current mode and down to previous mode
  help      Description of the interactive help system
  list      Print command list
  quit      Exit current mode and down to previous mode
  show      Show running system information
  terminal  Set terminal line parameters
  who       Display who is on vty
```

We provide examples with some of the [ospf6d](https://buildbot.quagga.net/docs/nightly/quagga/OSPFv3.html#OSPFv3) commands of [GNU Zebra 1.2.1](https://buildbot.quagga.net/docs/nightly/quagga/index.html). We start with the commands that provide information about the state of OSPFv3 and the link state database.

A first useful command is `show ipv6 ospf6 interface`. It provides information about each of the router's interfaces.

```console
rb> show ipv6 ospf6 interface 
lo is up, type LOOPBACK
  Interface ID: 1
   OSPF not enabled on this interface
rb-eth0 is up, type BROADCAST
  Interface ID: 2
  Internet Address:
    inet6: 2001:2345:7::b/64
    inet6: fe80::88fa:4cff:fe0d:6b71/64
  Instance ID 0, Interface MTU 1500 (autodetect: 1500)
  MTU mismatch detection: enabled
  Area ID 0.0.0.0, Cost 5
  State DR, Transmit Delay 1 sec, Priority 10
  Timer intervals configured:
   Hello 1, Dead 3, Retransmit 5
  DR: 0.0.0.3 BDR: 0.0.0.2
  Number of I/F scoped LSAs is 2
    0 Pending LSAs for LSUpdate in Time 00:00:00 [thread off]
    0 Pending LSAs for LSAck in Time 00:00:00 [thread off]
rb-eth1 is up, type BROADCAST
  Interface ID: 3
  Internet Address:
    inet6: 2001:2345:5::b/64
    inet6: fe80::54fc:e4ff:fe5c:121b/64
  Instance ID 0, Interface MTU 1500 (autodetect: 1500)
  MTU mismatch detection: enabled
  Area ID 0.0.0.0, Cost 1
  State BDR, Transmit Delay 1 sec, Priority 10
  Timer intervals configured:
   Hello 1, Dead 3, Retransmit 5
  DR: 0.0.0.4 BDR: 0.0.0.3
  Number of I/F scoped LSAs is 2
    0 Pending LSAs for LSUpdate in Time 00:00:00 [thread off]
    0 Pending LSAs for LSAck in Time 00:00:00 [thread off]
rb-eth2 is up, type BROADCAST
  Interface ID: 4
  Internet Address:
    inet6: 2001:2345:6::b/64
    inet6: fe80::70ad:1fff:fe99:8938/64
  Instance ID 0, Interface MTU 1500 (autodetect: 1500)
  MTU mismatch detection: enabled
  Area ID 0.0.0.0, Cost 5
  State BDR, Transmit Delay 1 sec, Priority 10
  Timer intervals configured:
   Hello 1, Dead 3, Retransmit 5
  DR: 0.0.0.5 BDR: 0.0.0.3
  Number of I/F scoped LSAs is 2
    0 Pending LSAs for LSUpdate in Time 00:00:00 [thread off]
    0 Pending LSAs for LSAck in Time 00:00:00 [thread off]

```

OSPFv3 is not used on the loopback interface. The interface `rb-eth0`
is attached to router `ra`. It has been configured with an IGP metric (Cost) of
5. On this interface, `rb` uses the public address `2001:2345:7::b/64` and
`fe80::88fa:4cff:fe0d:6b71/64` as its loopback address. The line 
`   Hello 1, Dead 3, Retransmit 5` indicates that this interface is
configured to send a Hello packet every second and considers the link to
be down when three Hello packets have been sent without any response. Similar information can be inferred for the other interfaces. Interface `rb-eth1`, which is attached
to router `rc`. 

Another useful command is `show ipv6 ospf6 spf tree` which provides an ASCII 
representation of the shortest path tree rooted at `rb`.

```console
rb> show ipv6 ospf6 spf tree 
+-0.0.0.3 [0]
   +-0.0.0.3 Net-ID: 0.0.0.2 [5]
   +-0.0.0.4 Net-ID: 0.0.0.3 [1]
   |  +-0.0.0.4 [1]
   |     +-0.0.0.4 Net-ID: 0.0.0.2 [2]
   |     |  +-0.0.0.2 [2]
   |     +-0.0.0.5 Net-ID: 0.0.0.3 [2]
   |        +-0.0.0.5 [2]
   +-0.0.0.5 Net-ID: 0.0.0.2 [5]
```

With OSPFv3, each router is assigned a RouterId. This identifier is encoded as
a 32 bits identifier and usually shown in dotted decimal format as an IPv4 address.
These identifiers are usually configured by the network operators. Each router must 
have a unique RouterId. In the emulated network, `rb` is identified by `0.0.0.3`.
Router `rb` has identifier `0.0.0.2`, ... 

Finally, the ` show ipv6 ospf6 database detail` command provides the detailed content of the link state database. OSPFv3 supports different Link State Attributes. A detailed
explanation of these attributes is outside the scope of this post, but here are
a few examples:

```console

Age: 1552 Type: Router
Link State ID: 0.0.0.0
Advertising Router: 0.0.0.2
LS Sequence Number: 0x8000000d
CheckSum: 0xb82f Length: 56
Duration: 00:25:50
    Bits: -------- Options: --|R|-|--|E|V6
    Type: Transit-Network Metric: 5
    Interface ID: 0.0.0.3
    Neighbor Interface ID: 0.0.0.2
    Neighbor Router ID: 0.0.0.3
    Type: Transit-Network Metric: 1
    Interface ID: 0.0.0.4
    Neighbor Interface ID: 0.0.0.2
    Neighbor Router ID: 0.0.0.4

Age:  310 Type: Network
Link State ID: 0.0.0.2
Advertising Router: 0.0.0.3
LS Sequence Number: 0x80000005
CheckSum: 0x3cde Length: 32
Duration: 00:05:09
     Options: --|R|-|--|E|V6
     Attached Router: 0.0.0.3
     Attached Router: 0.0.0.2
 
Age:  391 Type: Intra-Prefix
Link State ID: 0.0.0.0
Advertising Router: 0.0.0.2
LS Sequence Number: 0x80000007
CheckSum: 0x5cd4 Length: 44
Duration: 00:06:29
     Number of Prefix: 1
     Reference: Router Id: 0.0.0.0 Adv: 0.0.0.2
     Prefix Options: --|--|--|--
     Prefix: 2001:2345:1::/64

Age:  317 Type: Link
Link State ID: 0.0.0.3
Advertising Router: 0.0.0.2
LS Sequence Number: 0x80000006
CheckSum: 0x82e5 Length: 44
Duration: 00:05:15
     Priority: 10 Options: --|R|-|--|E|V6
     LinkLocal Address: fe80::a476:88ff:feb7:f2ed
     Number of Prefix: 0

```

The Router LSA provide information about a specific router inside the network. The Network LSA describes a network that connects two or more nodes. The Link LSA details a link between two routers and the `Intra-Prefix` LSA attaches an IPv6 prefix to a router.

We can also use the emulated network to observe the OSPFv3 packets that are exchanged
between the routers. For this, we disable the inter-router links with the following
commands:

```console
mininet> ra ip link set dev ra-eth1 down
mininet> ra ip link set dev ra-eth0 down
mininet> rb ip link set dev rb-eth1 down
mininet> rb ip link set dev rb-eth2 down
mininet> re ip link set dev re-eth1 down
```

At this point, all routers are isolated. We can verify `h1` and `h2` cannot
exchange packets:

```console
mininet> h1 traceroute6 2001:2345:2::2
traceroute to 2001:2345:2::2 (2001:2345:2::2) from 2001:2345:1::1, 30 hops max, 16 byte packets
 1  2001:2345:1::a (2001:2345:1::a)  0.096 ms !N  0.062 ms !N  0.035 ms !N
mininet> h2 traceroute6 2001:2345:1::1
traceroute to 2001:2345:1::1 (2001:2345:1::1) from 2001:2345:2::2, 30 hops max, 16 byte packets
 1  2001:2345:2::e (2001:2345:2::e)  0.056 ms !N  0.062 ms !N  0.047 ms !N
```

The `!N` mark shown in the `traceroute6` output indicates that the host received an
ICMPv6 Network Unreachable message instead of the expected HopLimit Exceeded.

Let us now observe what happens when we enable the link between `ra` and `rb`. 
This link has been shutdown on router `ra`. Before reactivating this link,
the start a `tcpdump` packet capture with `tcpdump -i rb-eth0 -n -vvv`.

We can now reactivate the link with `ra ip link set dev ra-eth0 up`. On router `rb`, we
first observe the transmission of its Hello packet :

```console
18:55:20.221431 IP6 (class 0xc0, flowlabel 0xc9f1b, hlim 1, next-header OSPF (89) payload length: 36) fe80::88fa:4cff:fe0d:6b71 > ff02::5: OSPFv3, Hello, length 36
        Router-ID 0.0.0.3, Backbone Area
        Options [V6, External, Router]
          Hello Timer 1s, Dead Timer 3s, Interface-ID 0.0.0.2, Priority 10
          Neighbor List:
```

This Hello packet is sent towards the `ff02::5` multicast address which is [reserved
for OSPF](https://www.iana.org/assignments/ipv6-multicast-addresses/ipv6-multicast-addresses.xhtml) by [IANA](https://www.iana.org).

Router `rb` replies shortly after:

```console
18:55:21.643273 IP6 (class 0xc0, flowlabel 0xd37a3, hlim 1, next-header OSPF (89) payload length: 36) fe80::a476:88ff:feb7:f2ed > ff02::5: OSPFv3, Hello, length 36
        Router-ID 0.0.0.2, Backbone Area
        Options [V6, External, Router]
          Hello Timer 1s, Dead Timer 3s, Interface-ID 0.0.0.3, Priority 10
          Neighbor List:
```

After having received this Hello packet, router `rb` updates its own Hello packet and resends it. 

```console
18:55:22.224464 IP6 (class 0xc0, flowlabel 0xc9f1b, hlim 1, next-header OSPF (89) payload length: 40) fe80::88fa:4cff:fe0d:6b71 > ff02::5: OSPFv3, Hello, length 40
        Router-ID 0.0.0.3, Backbone Area
        Options [V6, External, Router]
          Hello Timer 1s, Dead Timer 3s, Interface-ID 0.0.0.2, Priority 10
          Neighbor List:
            0.0.0.2
```

The next step is the transmission of the Database Description packet :

```console

18:55:23.227519 IP6 (class 0xc0, flowlabel 0xd2c53, hlim 64, next-header OSPF (89) payload length: 28) fe80::88fa:4cff:fe0d:6b71 > fe80::a476:88ff:feb7:f2ed: OSPFv3, Database Description, length 28
        Router-ID 0.0.0.3, Backbone Area
        Options [V6, External, Router], DD Flags [Init, More, Master], MTU 1500, DD-Sequence 0x000159c1

```

Router `ra` also sends its own Database Description packet:

```console
18:55:24.644380 IP6 (class 0xc0, flowlabel 0xd2c53, hlim 64, next-header OSPF (89) payload length: 28) fe80::a476:88ff:feb7:f2ed > fe80::88fa:4cff:fe0d:6b71: OSPFv3, Database Description, length 28
        Router-ID 0.0.0.2, Backbone Area
        Options [V6, External, Router], DD Flags [Init, More, Master], MTU 1500, DD-Sequence 0x000159c3
```

Later, router `ra` advertises the list of its Link State Attributes. Since we've shutdown some of the
links recently, there is still some information in the link state database that has not
yet expired. 

```console

18:55:28.235165 IP6 (class 0xc0, flowlabel 0xd2c53, hlim 64, next-header OSPF (89) payload length: 328) fe80::a476:88ff:feb7:f2ed > fe80::88fa:4cff:fe0d:6b71: OSPFv3, Database Description, length 328
        Router-ID 0.0.0.2, Backbone Area
        Options [V6, External, Router], DD Flags [none], MTU 1500, DD-Sequence 0x000159c1
          Advertising Router 0.0.0.2, seq 0x80000008, age 7s, length 24
            Link LSA (8), Link Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.3, seq 0x80000003, age 1427s, length 36
            Link LSA (8), Link Local Scope, LSA-ID 0.0.0.2
          Advertising Router 0.0.0.2, seq 0x80000012, age 27s, length 4
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.3, seq 0x8000000e, age 540s, length 4
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.4, seq 0x8000000b, age 546s, length 20
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.5, seq 0x80000005, age 1420s, length 36
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.4, seq 0x80000003, age 1424s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.5, seq 0x80000003, age 1425s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.2
          Advertising Router 0.0.0.5, seq 0x80000003, age 1420s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.2, seq 0x80000007, age 1423s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.3, seq 0x80000001, age 37s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.4, seq 0x80000003, age 1424s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.5, seq 0x80000007, age 1420s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.5, seq 0x80000003, age 1425s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.2
          Advertising Router 0.0.0.5, seq 0x80000003, age 1420s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.3

```

Router `rb` also sends its full Database Description.

```console
18:55:28.235641 IP6 (class 0xc0, flowlabel 0xd2c53, hlim 64, next-header OSPF (89) payload length: 40) fe80::88fa:4cff:fe0d:6b71 > fe80::a476:88ff:feb7:f2ed: OSPFv3, LS-Request, length 40
        Router-ID 0.0.0.3, Backbone Area
          Advertising Router 0.0.0.2
            Link LSA (8), Link Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.2
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
18:55:28.235864 IP6 (class 0xc0, flowlabel 0xd2c53, hlim 64, next-header OSPF (89) payload length: 328) fe80::88fa:4cff:fe0d:6b71 > fe80::a476:88ff:feb7:f2ed: OSPFv3, Database Description, length 328
        Router-ID 0.0.0.3, Backbone Area
        Options [V6, External, Router], DD Flags [Master], MTU 1500, DD-Sequence 0x000159c2
          Advertising Router 0.0.0.2, seq 0x80000007, age 29s, length 24
            Link LSA (8), Link Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.3, seq 0x80000003, age 1426s, length 36
            Link LSA (8), Link Local Scope, LSA-ID 0.0.0.2
          Advertising Router 0.0.0.2, seq 0x80000010, age 588s, length 4
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.3, seq 0x80000010, age 27s, length 4
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.4, seq 0x8000000b, age 545s, length 20
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.5, seq 0x80000005, age 1419s, length 36
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.4, seq 0x80000003, age 1425s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.5, seq 0x80000003, age 1424s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.2
          Advertising Router 0.0.0.5, seq 0x80000003, age 1419s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.2, seq 0x80000007, age 1424s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.3, seq 0x80000002, age 9s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.4, seq 0x80000003, age 1425s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.3
          Advertising Router 0.0.0.5, seq 0x80000007, age 1419s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.0
          Advertising Router 0.0.0.5, seq 0x80000003, age 1424s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.2
          Advertising Router 0.0.0.5, seq 0x80000003, age 1419s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.3

```

Then router `ra` confirms that the link between itself (RouterId `0.0.0.2`) and router 
`rb` (RouterId `0.0.0.3`) is active.

```console

18:55:28.235991 IP6 (class 0xc0, flowlabel 0xd37a3, hlim 1, next-header OSPF (89) payload length: 88) fe80::a476:88ff:feb7:f2ed > ff02::5: OSPFv3, LS-Update, length 88
        Router-ID 0.0.0.2, Backbone Area
          Advertising Router 0.0.0.2, seq 0x80000008, age 7s, length 24
            Link LSA (8), Link Local Scope, LSA-ID 0.0.0.3
              Options [V6, External, Router]
              Priority 10, Link-local address fe80::a476:88ff:feb7:f2ed, Prefixes 0:
          Advertising Router 0.0.0.2, seq 0x80000012, age 27s, length 4
            Router LSA (1), Area Local Scope, LSA-ID 0.0.0.0
              Options [V6, External, Router], RLA-Flags [none]
18:55:28.236010 IP6 (class 0xc0, flowlabel 0xd2c53, hlim 64, next-header OSPF (89) payload length: 96) fe80::a476:88ff:feb7:f2ed > fe80::88fa:4cff:fe0d:6b71: OSPFv3, LS-Update, length 96
        Router-ID 0.0.0.2, Backbone Area
          Advertising Router 0.0.0.3, seq 0x80000005, age 3600s, length 12
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.2
              Options [V6, External, Router]
              Connected Routers:
                0.0.0.3
                0.0.0.2
          Advertising Router 0.0.0.3, seq 0x80000005, age 3600s, length 24
            Intra-Area Prefix LSA (9), Area Local Scope, LSA-ID 0.0.0.2
            Network LSA (2), Area Local Scope, LSA-ID 0.0.0.2
              Prefixes 1:
                2001:2345:7::/64, metric 0

```

This is only a small subset of all the packets that are exchanged by OSPFv3. OSPFv3 
can be a chatty protocol. In our emulated network, we use emulated Ethernet LANs, which implies that
routers will elect a Designated Router on each link. Here are two packet traces which can be
used as a starting point to explore the behaviour of OSPFv3 in this network.

 - the [first trace]({{ site.baseurl }}/files/ospfv3-rb.pcap)) was collected on router `rb` after having enabled the link between `ra` and `rb`
 - the [second trace]({{ site.baseurl }}/files/ospfv3-rb2.pcap) was collected on the same router while we reactivated all links inside the network in the following order

```console
mininet> ra ip link set dev ra-eth1 up
mininet> ra ip link set dev ra-eth0 up
mininet> rb ip link set dev rb-eth1 up
mininet> rb ip link set dev rb-eth2 up
mininet> re ip link set dev re-eth1 up
```

At this point, the entire network is up again and all nodes can ping each other.

Suggestions on exercises to let students learn OSPFv3 are welcome as [pull requests](https://github.com/obonaventure/RoutingExamples/pulls) or [issues](https://github.com/obonaventure/RoutingExamples/issues) in the [RoutingExamples](https://github.com/obonaventure/RoutingExamples/) project on GitHub.


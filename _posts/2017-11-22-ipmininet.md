---
layout: post
title: Experimenting with Mininet and IPv6 routes
tag: IPv6, Mininet
author: Olivier Bonaventure
---

When students discover IPv6, they usually start playing with static routes to understand how routing tables are built. At UCL, we've used
a variety of techniques to let the students understand routing tables. A first approach is to simply use the blackboard and let
the students analyse routing tables and explain how packets will
be forwarded in a given network. This works well, but students
often ask for additional exercises to practice before the exam. Another approach is to use [netkit](https://netkit-ng.github.io). [netkit](https://netkit-ng.github.io) was designed by researchers at Roma3 University as an experimental learning tool. It relies on
User Mode Linux to run a Linux kernels as processes on a virtual
machine. Several student labs were provided by the netkit authors. We have used it in the past, but the project does not seem to make progress anymore. A third approach is to use [Mininet](https://mininet.org). [Mininet](https://mininet.org) is an emulation framework developed at Stanford University that leverages the namespaces features of recent Linux kernel. With those features, a single Linux kernel can support a variety of routers and hosts interconnected by virtual links. [Mininet](https://mininet.org) has been used by various universities as an educational tool, but unfortunately it was designed with IPv4 in mind while
[Computer Networking : Principles, Protocols and Practice](http://cnp3book.info.ucl.ac.be) has been focussed on IPv6.

[Olivier Tilmans](https://inl.info.ucl.ac.be/otilmans) has recently
developed one missing piece to enable students to use
[Mininet](https://mininet.org) to experiment with IPv6. His
[IPMininet](https://github.com/oliviertilmans/ipmininet) python module
provides the classes that are required to automatically configure IPv6
networks with various forms of routing. It is available from PyPi from
[https://pypi.python.org/ipmininet](https://pypi.python.org/pypi/ipmininet).

The syntax of [IPMininet](https://github.com/oliviertilmans/ipmininet) is relatively simple and can be learned by looking at a few examples.

Let us start our exploration of IPv6 routing with simple network topology that contains three hosts and three routers and uses static routes.
```text
a ---- r1 ---- r2 ---- r3 ---- c
                +
                b 
```

Although [IPMininet](https://github.com/oliviertilmans/ipmininet) can assign prefixes and addresses automatically, we use manually assigned addresses in this example.

We use five `/64` IPv6 prefixes in this network topology:

 - `2001:89ab:12::/64` on the link between `r1` and `r2`
 - `2001:89ab:23::/64` on the link between `r2` and `r3`
 - `2001:7ab:1::/64` on the link between host `a` and `r1` 
 - `2001:7ab:2::/64` on the link between host `b` and `r2`
 - `2001:7ab:3::/64` on the link between host `c` and `r3`

Given the network topology, we install default routes on `r1` and
`r3` via `r2`. Router `r1` has static routes to reach `2001:7ab:1::/64` via `r1` and `2001:7ab:3::/64` via `r3`.
 
With these IP prefixes and the network topology, we can now
use IPMininet to create the topology and assign the addresses.

We start by creating the objects that correspond to the static routes on the three routers.

```python
 r1_routes = [StaticRoute("::/0", "2001:89ab:12::2")]
 r3_routes = [StaticRoute("::/0", "2001:89ab:23::1")]
 r2_routes = [StaticRoute("2001:7ab:1::/64", "2001:89ab:12::1"),
              StaticRoute("2001:7ab:3::/64", "2001:89ab:23::2")]
```

We can now create the three routers.

```python
 r1 = self.addRouter_v6('r1', r1_routes)
 r2 = self.addRouter_v6('r2', r2_routes)
 r3 = self.addRouter_v6('r3', r3_routes)
```

And the links between the pairs of routers. In these lines, `params1` (resp. `params2`) corresponds to the configuration of the interface on the first (resp. second) node.

```python
 self.addLink(r1, r2, params1={"ip": "2001:89ab:12::1/64"},
                      params2={"ip": "2001:89ab:12::2/64"})
 self.addLink(r2, r3, params1={"ip": "2001:89ab:23::1/64"},
                      params2={"ip": "2001:89ab:23::2/64"})
```

The configuration of the links between the routers and the hosts follows a similar pattern.

```python
 self.addLink(r1, self.addHost('a'),
                  params1={"ip": "2001:7ab:1::1/64"},
                  params2={"ip": "2001:7ab:1::a/64"})
 self.addLink(r2, self.addHost('b'),
                  params1={"ip": "2001:7ab:2::1/64"},
                  params2={"ip": "2001:7ab:2::b/64"})
 self.addLink(r3, self.addHost('c'),
                  params1={"ip": "2001:7ab:3::1/64"},
                  params2={"ip": "2001:7ab:3::c/64"})
```

The entire script is available from [https://github.com/obonaventure/RoutingExamples/blob/master/static/simple.py](https://github.com/obonaventure/RoutingExamples/blob/master/static/simple.py)

To help students to start using [IPMininet](https://github.com/oliviertilmans/ipmininet), [Mathieu Jadin](https://inl.info.ucl.ac.be/mjadin) has created a [Vagrant box](https://www.vagrantup.com/docs/index.html) that launches a Ubuntu virtual machine will all the required software. You can access the VagrantFile and the associated puppet manifests from [https://github.com/obonaventure/RoutingExamples](https://github.com/obonaventure/RoutingExamples)

Here is a simple example of the utilisation of this Vagrant box.

We start the network topology shown above with the `sudo python ipmininet_example.py` command. It launches the Mininet interactive shell that provides several useful commands:

```console
mininet> help

Documented commands (type help <topic>):
========================================
EOF    gterm  iperf     links   pingall       ports  route   time 
dpctl  help   iperfudp  net     pingallfull   px     sh      x    
dump   intfs  ips       nodes   pingpair      py     source  xterm
exit   ip     link      noecho  pingpairfull  quit   switch

You may also send a command to a node using:
  <node> command {args}
For example:
  mininet> h1 ifconfig

The interpreter automatically substitutes IP addresses
for node names when a node is the first arg, so commands
like
  mininet> h2 ping h3
should work.

Some character-oriented interactive commands require
noecho:
  mininet> noecho h2 vi foo.py
However, starting up an xterm/gterm is generally better:
  mininet> xterm h2

mininet>
```

Some of the standard mininet commands assume the utilisation of
IPv4 and do not have a direct IPv6 equivalent. Here are some useful commands.

The `nodes` command lists the routers and hosts that have been created in the mininet topology.

```console
mininet> nodes
available nodes are: 
a b c r1 r2 r3
```

The `links` command lists the links that have been instantiated and
shows that mapping between the named interfaces on each node.

```console
mininet> links
r1-eth1<->a-eth0 (OK OK)
r1-eth0<->r2-eth0 (OK OK)
r2-eth2<->b-eth0 (OK OK)
r2-eth1<->r3-eth0 (OK OK)
r3-eth1<->c-eth0 (OK OK)
```

It is possible to execute any of the standard Linux commands to configure the TCP/IP stack on any of the hosts by prefixing the command with the corresponding host. Remember to always specify
`inet6` as the address family to retrieve the IPv6 information.

```console
mininet> a ip -f inet6 link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: a-eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether d6:a0:57:60:09:0a brd ff:ff:ff:ff:ff:ff link-netnsid 0
mininet> a ip -f inet6 address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 state UNKNOWN qlen 1
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: a-eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 2001:7ab:1::a/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::d4a0:57ff:fe60:90a/64 scope link 
       valid_lft forever preferred_lft forever
mininet> a ip -f inet6 route
2001:7ab:1::/64 dev a-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev a-eth0  proto kernel  metric 256  pref medium
default via 2001:7ab:1::1 dev a-eth0  metric 1024  pref medium
```

```console
mininet> a ip -f inet6 route
2001:7ab:1::/64 dev a-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev a-eth0  proto kernel  metric 256  pref medium
default via 2001:7ab:1::1 dev a-eth0  metric 1024  pref medium
mininet> r2 ip -f inet6 route
2001:7ab:1::/64 via 2001:89ab:12::1 dev r2-eth0  proto zebra  metric 20  pref medium
2001:7ab:2::/64 dev r2-eth2  proto kernel  metric 256  pref medium
2001:7ab:3::/64 via 2001:89ab:23::2 dev r2-eth1  proto zebra  metric 20  pref medium
2001:89ab:12::/64 dev r2-eth0  proto kernel  metric 256  pref medium
2001:89ab:23::/64 dev r2-eth1  proto kernel  metric 256  pref medium
fe80::/64 dev r2-eth0  proto kernel  metric 256  pref medium
fe80::/64 dev r2-eth2  proto kernel  metric 256  pref medium
fe80::/64 dev r2-eth1  proto kernel  metric 256  pref medium
```

Another useful command is `xterm node` that allows to launch a terminal on the specified `node`. This gives you a interactive shell
on any node. You can use it to capture packets with `tcpdump`. As an example, let us use `traceroute6` to trace the path followed by packets from host `a` towards the IPv6 address of host `v`, i.e. ` 2001:7ab:3::c`. The output of this command shows that the path passes through routers `r1`, `r2` and `r3`.


```console
mininet> a traceroute6 -q 1 2001:7ab:3::c
traceroute to 2001:7ab:3::c (2001:7ab:3::c) from 2001:7ab:1::a, 30 hops max, 16 byte packets
 1  2001:7ab:1::1 (2001:7ab:1::1)  0.105 ms
 2  2001:89ab:12::2 (2001:89ab:12::2)  1.131 ms
 3  2001:89ab:23::2 (2001:89ab:23::2)  0.845 ms
 4  2001:7ab:3::c (2001:7ab:3::c)  0.254 ms
```

When debugging a network, it can be interesting to capture packets on specific links to check that they follow the expected path. In this simple network, the best place to capture packets is on router `r2`. This router has two interfaces: `r2-eth0` connected to `r1` and `r2-eth1` connected to `r3`. If you use `tcpdump` without any
filter, you will capture the packets generated by `xterm`. To capture packets, you need to specify precise filters that will match the packets of interest. For `traceroute6`, you need to match the IPv6 packets that contain UDP segments and some ICMPv6 packets. The script below provides a simple filter that you can reuse. It takes one argument: the name of the interface on which `tcpdump` needs to run. 

```bash
#!/bin/bash

tcpdump -v -i $1 -n '(ip6 && udp) || (icmp6  && (ip6[40] == 1 || ip6[40]==3))'
```

Here are the packets captured by `tcpdump` on the two interfaces of router `r2`.

```console
#sh dump-traceroute.sh r2-eth0
tcpdump: listening on r2-eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
13:26:20.738814 IP6 (flowlabel 0xee1ca, hlim 1, next-header UDP (17) payload length: 24) 2001:7ab:1::a.60819 > 2001:7ab:3::c.33434: [bad udp cksum 0x4f9b -> 0xf554!] UDP, length 16
13:26:20.738835 IP6 (flowlabel 0x467c2, hlim 64, next-header ICMPv6 (58) payload length: 72) 2001:89ab:12::2 > 2001:7ab:1::a: [icmp6 sum ok] ICMP6, time exceeded in-transit for 2001:7ab:3::c
13:26:20.739280 IP6 (flowlabel 0xee1ca, hlim 2, next-header UDP (17) payload length: 24) 2001:7ab:1::a.60819 > 2001:7ab:3::c.33434: [bad udp cksum 0x4f9b -> 0x2152!] UDP, length 16
13:26:20.739310 IP6 (flowlabel 0x6dc40, hlim 63, next-header ICMPv6 (58) payload length: 72) 2001:89ab:23::2 > 2001:7ab:1::a: [icmp6 sum ok] ICMP6, time exceeded in-transit for 2001:7ab:3::c
13:26:20.740206 IP6 (flowlabel 0xee1ca, hlim 3, next-header UDP (17) payload length: 24) 2001:7ab:1::a.60819 > 2001:7ab:3::c.33434: [bad udp cksum 0x4f9b -> 0x934d!] UDP, length 16
13:26:20.740297 IP6 (flowlabel 0xe744c, hlim 62, next-header ICMPv6 (58) payload length: 72) 2001:7ab:3::c > 2001:7ab:1::a: [icmp6 sum ok] ICMP6, destination unreachable, unreachable port, 2001:7ab:3::c udp port 33434
```

```console
# sh dump-traceroute.sh r2-eth1
tcpdump: listening on r2-eth1, link-type EN10MB (Ethernet), capture size 262144 bytes
13:26:20.739288 IP6 (flowlabel 0xee1ca, hlim 1, next-header UDP (17) payload length: 24) 2001:7ab:1::a.60819 > 2001:7ab:3::c.33434: [bad udp cksum 0x4f9b -> 0x2152!] UDP, length 16
13:26:20.739307 IP6 (flowlabel 0x6dc40, hlim 64, next-header ICMPv6 (58) payload length: 72) 2001:89ab:23::2 > 2001:7ab:1::a: [icmp6 sum ok] ICMP6, time exceeded in-transit for 2001:7ab:3::c
13:26:20.740221 IP6 (flowlabel 0xee1ca, hlim 2, next-header UDP (17) payload length: 24) 2001:7ab:1::a.60819 > 2001:7ab:3::c.33434: [bad udp cksum 0x4f9b -> 0x934d!] UDP, length 16
13:26:20.740290 IP6 (flowlabel 0xe744c, hlim 63, next-header ICMPv6 (58) payload length: 72) 2001:7ab:3::c > 2001:7ab:1::a: [icmp6 sum ok] ICMP6, destination unreachable, unreachable port, 2001:7ab:3::c udp port 33434
```

We clearly observe that `traceroute6` sends UDP segments with a decreasing HopLimit. On the other hand, the intermediate routers reply with ICMPv6 time exceeded messages while the destination returns an ICMPv6 port unreachable message back to the source.

This is a first example with static IPv6 routes which could bootstrap students who are learning IPv6 routing. Other examples will follow. Suggestions are welcome as [issues](https://github.com/obonaventure/RoutingExamples/issues/new) or better [pull requests](https://github.com/obonaventure/RoutingExamples/pulls) on [https://github.com/obonaventure/RoutingExamples](https://github.com/obonaventure/RoutingExamples)



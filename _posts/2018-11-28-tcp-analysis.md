---
layout: post
title: A first analysis of a TCP server
tag: TCP, wireshark, scapy
author: Olivier Bonaventure
---

Networking students can learn a lot about Internet protocols by analyzing how they are actually deployed. For several years, Computer Science students at [UCLouvain](https://www.uclouvain.be) have analyzed different websites within their introductory networking course. This project considers several key Internet protocols, DNS, HTTP, TLS and TCP. In this post, we briefly analyze how TCP is used on some web sites as a starting point for these students.

The simplest way to analyze the TCP configuration of a distant web server is to establish a connection towards this server, e.g. with [firefox](https://www.mozilla.org/en-US/firefox/new/) while collecting all the exchanged packets with [wireshark](https://www.wireshark.org). However, while doing so, it is important to ensure that there are no other applications that exchange packets during the collection of the trace.

As an example, let us look at the packet exchanged between a Mac using `Darwin Kernel Version 17.7.0` and [http://cnp3book.info.ucl.ac.be](http://cnp3book.info.ucl.ac.be). The first packet was sent by the client, it contains the TCP extensions that it proposes to the server. We observe that it includes an MSS size (1440 bytes), the Window Scale option (with a scaling factor of 5) and the Timestamp option defined in [RFC1323](https://tools.ietf.org/html/rfc1323), the SACK permitted option defined in [RFC2018](https://tools.ietf.org/html/rfc2018). The No-Operation and the End of Option List options are used as padding to ensure that the extended TCP header spans a multiple of four bytes. 

![First TCP segment]({{ site.baseurl }}/images/tcp-1.png)

An interesting point about the SYN sent by the client is that the ECN-Echo and the CWR flags are set. This is an indication that the client proposes to use Explicit Congestion Notification defined in [RFC3168](https://tools.ietf.org/html/rfc3168). The analysis of the server starts with the SYN+ACK that it returns.

![Second TCP segment]({{ site.baseurl }}/images/tcp-2.png)

This SYN+ACK indicates that the server is willing to use Explicit Congestion Notification since the ECN Echo bit is set. The server also supports the Timestamp, Window Scale and Selective Acknowledgments options and its MSS has a length of 1440 bytes. It is interesting to note that the Windows Scale factor proposed by the server is larger than the factor proposed by the client.

This wireshark output is only the starting point of the analysis of the TCP configuration because the returned SYN+ACK depends on the options that were sent by the client in its SYN.

Another interesting point is to try to estimate the initial congestion window configured on the server. A naive approach is to find a sufficiently large web object that it available on this server and collect a trace while retrieving it. A simple example is one image such as [http://cnp3book.info.ucl.ac.be/exercises/html/_images/cnp3.png](http://cnp3book.info.ucl.ac.be/exercises/html/_images/cnp3.png). When this large download starts, we observe that the server only sends three segments before waiting for an acknowledgment.


![Large download]({{ site.baseurl }}/images/tcp-3.png)


Unfortunately, there are situations where this estimation will not be correct, notably when the server is close to the client. Researchers have proposed a [detailed methodology](https://conferences.sigcomm.org/imc/2017/papers/imc17-final43.pdf) to infer this initial congestion window. Implementing it is outside the scope of a simple course project, but additional results are available on [https://iw.netray.io](https://iw.netray.io).

Another interesting point to analyze are the TCP extensions that are supported by the server. Besides the extensions that we have already identified, it would be interesting to check whether the server supports newer extensions such as Multipath TCP [RFC6824](https://tools.ietf.org/html/rfc6824) or TCP Fast Open [RFC7413](https://tools.ietf.org/html/rfc7413). An interesting tool to carry such an analysis is [scapy](https://scapy.net). [scapy](https://scapy.net) provides a set of python methods that allow to easily craft and transmit IP packets that contain TCP segments. It works on Linux, Windows and MacOS and thus should be easy to use.

Since [scapy](https://scapy.net) crafts IP packets, it needs to be executed with administrator privileges. The [scapy documentation](https://scapy.readthedocs.io/en/latest/) contains a detailed [tutorial](https://scapy.readthedocs.io/en/latest/#interactive-tutorial) that describes many advanced features. Here are a few useful ones.

First, [scapy](https://scapy.net) can be used to create any type of packet. The simplest example is shown below.

```
>>> a=IPv6()
>>> a.show()
###[ IPv6 ]###
version= 6
tc= 0
fl= 0
plen= None
nh= No Next Header
hlim= 64
src= ::1
dst= ::1
```
The first line creates an empty IPv6 packet whose header fields have been initialized with default values. Each of the fields shown above can be set when constructing the IPv6 packet. For example, `a=IPv6(dst="2001:6a8:308f:1:216:3eff:fec5:c815")` creates an IPv6 packet with the specified destination address.

It is also possible to create a TCP segment inside an IPv6 packet as follows:

```
>>> a=IPv6(dst="2001:6a8:308f:1:216:3eff:fec5:c815")/TCP()
>>> a.show()
###[ IPv6 ]###
version= 6
tc= 0
fl= 0
plen= None
nh= TCP
hlim= 64
src= 2001:41d0:a:ffcf::1
dst= 2001:6a8:308f:1:216:3eff:fec5:c815
###[ TCP ]###
sport= ftp_data
dport= http
seq= 0
ack= 0
dataofs= None
reserved= 0
flags= S
window= 8192
chksum= None
urgptr= 0
options= {}
```

Again, we can force any value for the fields of the TCP header. An interesting feature of scapy is its ability to send a single IP packet and wait for a response.

```
>>> sr1(IPv6(dst="2001:6a8:308f:1:216:3eff:fec5:c815")/TCP(dport=80,flags="S",options=[('MSS',(1440)),('TFO',(0,0))]))
Begin emission:
.....Finished to send 1 packets.
.*
Received 7 packets, got 1 answers, remaining 0 packets
<IPv6  version=6L tc=0L fl=338520L plen=24 nh=TCP hlim=54 src=2001:6a8:308f:1:216:3eff:fec5:c815 dst=2001:41d0:a:ffcf::1 |<TCP  sport=http dport=ftp_data seq=2180813231 ack=1 dataofs=6L reserved=0L flags=SA window=28800 chksum=0x52c3 urgptr=0 options=[('MSS', 1440)] |>>
```

In the above example, we send a TCP SYN to `2001:6a8:308f:1:216:3eff:fec5:c815` on port 80 with the MSS option and the TFO option. We received a SYN+ACK, but without the TCP Fast Open option, which indicates that the server does not support this extension.

A similar test towards the IETF web site reveals that it supports this extension:

```
>>> sr1(IPv6(dst="www.ietf.org")/TCP(dport=443,flags="S",options=[('MSS',(1440)),('TFO',(0,0))]))
Begin emission:
.........Finished to send 1 packets.
..*
Received 12 packets, got 1 answers, remaining 0 packets
<IPv6  version=6L tc=0L fl=778959L plen=36 nh=TCP hlim=58 src=2606:4700:10::6814:155 dst=2001:41d0:a:ffcf::1 |<TCP  sport=https dport=ftp_data seq=992052241 ack=1 dataofs=9L reserved=0L flags=SA window=27200 chksum=0xf28d urgptr=0 options=[('MSS', 1360), ('TFO', (2556863396, 1710640485)), ('NOP', None), ('NOP', None)] |>>
```

Unfortunately, the current public version of scapy does not currently support Multipath TCP. It is thus not possible to test whether the remote server supports it.

It is also possible to check whether a server supports Explicit Congestion Notification by setting the ECN Echo and CWR flags in the SYN segment.

```
>>> sr1(IPv6(dst="www.ietf.org")/TCP(dport=443,flags="SEC",options=[('MSS',(1440)),('TFO',(0,0))]))
Begin emission:
................Finished to send 1 packets.
..*
Received 19 packets, got 1 answers, remaining 0 packets
<IPv6  version=6L tc=0L fl=201518L plen=36 nh=TCP hlim=58 src=2606:4700:10::6814:55 dst=2001:41d0:a:ffcf::1 |<TCP  sport=https dport=ftp_data seq=1860257081 ack=1 dataofs=9L reserved=0L flags=SAE window=27200 chksum=0x408d urgptr=0 options=[('MSS', 1360), ('TFO', (1962155903, 3428797548)), ('NOP', None), ('NOP', None)] |>>
```



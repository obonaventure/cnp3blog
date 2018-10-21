---
layout: post
title: Discussions on TCP timestamp
tag: TCP, Timestamp, IETF
author: Olivier Bonaventure
---

The TCP Timestamp option was proposed in [RFC1323](https://tools.ietf.org/html/rfc1323) in 1992 at the same time as the Window Scale option. There were two motivations for the initial TCP Timestamp option : improving round-trip-time estimation and protecting agains wrapped sequence numbers (PAWS). By adding timestamps to each packets, it becomes easier to estimate round-trip-times, especially when packets are lost because retransmissions of a packet carry different timestamps. The PAWS mechanism is less well understood. It is a direct consequence of the utilisation of 32 bits sequence numbers in TCP. TCP [RFC793](https://tools.ietf.org/html/rfc793) was designed under the assumption that the IP layer guarantees that a packet will not live in the network for more than 2 minutes (the Maximum Segment Lifetime). TCP's reliable transmission can be guaranteed provided that it does not use the same sequence number for different packets within MSL seconds. In 1981, with 32 bits sequence numbers, nobody thought that reusing the same sequence number over 2 minutes would become a problem. Today, this is a reality, even in wide area networks. PAWS [RFC7323](https://tools.ietf.org/html/rfc7323) solves this problem by using timestamps to detects spurious packets and prevent from problems where old packets are delayed within MSL seconds. It took more than a decade to reach a significant deployment of [RFC1323](https://tools.ietf.org/html/rfc1323).

In 2014, a revision of the specification was posted in [RFC7323](https://tools.ietf.org/html/rfc7323). This revision introduced an important modification to the handling of the timestamp option.

In [RFC1323](https://tools.ietf.org/html/rfc1323#section-3), the timestamps where optional. They could be negotiated at the beginning of the connection and used in any segment later:

```console
         A TCP may send the Timestamps option (TSopt) in an initial
         <SYN> segment (i.e., segment containing a SYN bit and no ACK
         bit), and may send a TSopt in other segments only if it re-
         ceived a TSopt in the initial <SYN> segment for the connection.
```

[RFC7323](https://tools.ietf.org/html/rfc7323#section-3) introduced an important modification to this rule:

```console
   A TCP MAY send the TSopt in an initial <SYN> segment (i.e., segment
   containing a SYN bit and no ACK bit), and MAY send a TSopt in
   <SYN,ACK> only if it received a TSopt in the initial <SYN> segment
   for the connection.

   Once TSopt has been successfully negotiated, that is both <SYN> and
   <SYN,ACK> contain TSopt, the TSopt MUST be sent in every non-<RST>
   segment for the duration of the connection, and SHOULD be sent in an
   <RST> segment (see Section 5.2 for details).  The TCP SHOULD remember
   this state by setting a flag, referred to as Snd.TS.OK, to one.  If a
   non-<RST> segment is received without a TSopt, a TCP SHOULD silently
   drop the segment.  A TCP MUST NOT abort a TCP connection because any
   segment lacks an expected TSopt.
```

There were many discussions on the [tcpm mailing list](https://www.ietf.org/mail-archive/web/tcpm) when [RFC7323](https://tools.ietf.org/html/rfc7323) was published. One of the concerns was that requiring the timestamp option in each packet was unnecessary when TCP extensions such as Multipath TCP [RFC6824](https://tools.ietf.org/html/rfc6824) are used since those extensions contain their own sequence numbers that provide PAWS.

In June, Yoshifumi Nishida documented his concerns in [draft-nishida-tcpm-disabling-paws-00](https://tools.ietf.org/html/draft-nishida-tcpm-disabling-paws-00). 
A few weeks ago, 26 years after the initial specification, he looked in publicly available packet traces to check the deployment of the TCP Timestamp options and found that [only 60-70% of the captured connections used this option](https://www.ietf.org/mail-archive/web/tcpm/current/msg11521.html). His post on the tcpm mailing list triggered an interesting discussion on the utilisation of this option. On one hand, Praveen Balasubramanian, who leads the TCP development within Microsoft, confirmed that Microsoft's stacks do not use this option by default, [stating](https://www.ietf.org/mail-archive/web/tcpm/current/msg11522.html) that *Windows doesn’t do timestamp option by default. It’s 10 byte per packet overhead for marginal benefits*. On the Linux side, Yuchung Cheng, [replied](https://www.ietf.org/mail-archive/web/tcpm/current/msg11529.html) that they saw four benefits for the TCP Timestamp option which is included in the Linux TCP stack since more than a decade:

```console
However I am sensing that the value of TCP timestamp options is
under-valued - Linux for example uses TS for many things:

1. RTT estimation on retransmission - this is crucial when a TCP is in
constant recovery mode such as encountering a traffic policer. Note
that the standard ACK time approach no longer works if any sequence
acked has been transmitted, thus RTO may stale during extended
recovery period.

2. Undo operations (i.e. TCP Eifel RFC4015) requires TCP timestamps -
being able to detect false reactions to losses is particularly key for
"loss-based" congestion control. Such false positives are common in
wireless or even wired networks due to all sorts of dark queues and L2
optimizations. While DSACK and F-RTO can also be used to detect false
recoveries - both of them take at least one more round-trip and/or
require new application data, while timestamp can detect false
retransmission instantly on the original ACK

3. Receiver buffer auto-tuning uses TCP ACKs' TS value to echo to
measure RTT from the receiver to tune the buffer size.

4. Potentially TCP timestamps being generated with known units, can be
used to gauge the data arrival rate. The measurement can be valuable
for congestion control b/c it's not subject to hectic ACK delays in
modern networks. I know folks at NetFlix is doing some deep research
into this aspect.
```

Surprisingly, PAWS does not seem to be considered as a strong use case for thr TCP timestamp option these days...


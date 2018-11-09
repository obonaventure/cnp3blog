---
layout: post
title: A new type of exercises to learn TCP
tags: [TCP, INGINIOUS]
author: Olivier Bonaventure
---

During the last summer, we brainstormed about new types of exercises
that could help students to better understand different networking
topics. For these new exercices, we wanted to leverage the
[INGINIOUS](https://www.inginious.org) code grading platform that is
being developed at [UCLouvain](https://www.uclouvain.be). The
[UDP socket](https://obonaventure.github.io/cnp3blog/udptest/) and
[TCP socket](https://obonaventure.github.io/cnp3blog/tcptest/)
exercises are good examples of the benefits of a flexible code grading
platform like [INGINIOUS](https://www.inginious.org).

Knowing that students gain a detailed understanding of the TCP header
by looking at packet traces,
[Fabien Duchêne](https://uclouvain.be/en/directories/fabien.duchene)
suggested to develop exercises where the students are exposed to a
packet trace where some fields of some packets are missing or those
packets are not displayed in the right order. The idea looked really
nice on paper, but it was not yet supported by
[INGINIOUS](https://www.inginious.org). Thanks to funding provided by 
[UCLouvain](https://www.uclouvain.be)'s
[OER Initiative](https://oer.uclouvain.be/jspui/),
[Maxime Piraux](https://uclouvain.be/fr/repertoires/maxime.piraux)
with the help of [Luis Tascon Gutierrez](https://github.com/TGLuis)
developed a nice
[INGINIOUS plugin](https://github.com/UCL-INGI/INGInious-problems-network-trace)
to develop this new type of exercises.

This plugin defines a new type of exercise that takes a packet trace
as input. It can then display the trace after having blanked some
fields that need to be recovered by the students. The plugin can also
shuffle the packets and ask the student to reorder them. More than a
dozen of such exercises are available on
[https://inginious.org/course/cnp3](https://inginious.org/course/cnp3)
and new ones can be easily created.

As an illustration of these new exercises, let us consider two of
them. The [first one](https://inginious.org/course/cnp3/tcp-syn-port),
asks the students to determine the values of the source and
destination ports in the first segment sent to establish a connection.

This exercises uses a simple packet trace containing two packets. This
packet trace is analysed by the plugin and represented in a format
that mimics Wireshark. The student needs to provide information about
the first packet (shown with the warning icon pointed by a red arrow)
and has to encode the source and destination ports.

![https://inginious.org/course/cnp3/tcp-syn-port]({{ site.baseurl }}/images/inginious-tcp-1.png)

The student can easily answer this question by looking at the second
packet of the trace that contains the source and destination ports.

![https://inginious.org/course/cnp3/tcp-syn-port]({{ site.baseurl }}/images/inginious-tcp-2.png) 

The student then simply needs to encode the missing port
numbers. Inginious will check them and provide feedback in case of
errors as illustrated below.

![https://inginious.org/course/cnp3/tcp-syn-port]({{ site.baseurl }}/images/inginious-tcp-3.png) 

From the teacher's viewpoint, it is simple to write these new packet
exercises. The main difficulty is to collect a packet trace containing
packets which can be understood by the students. Inginious provides a
simple web interface to encode the exercise. The first step is to
define the context of the exercises in restructured text format.

![https://inginious.org/course/cnp3/tcp-syn-port]({{ site.baseurl }}/images/inginious-tcp-4.png) 

Then, the teacher needs to fill a simple web form. Besides the text of
the exercise, the form contains four important fields:

 - the packet trace in libpcap format
 - the layers of the TCP/IP reference model that need to be exposed by
 the packet dissector. In this example, we only show the TCP headers,
 but it is also possible to expose the network layer and the
 application layer.
 - the fields of the packets that the students will have to guess. In
   this example, the student needs to guess the `Source port` and
   `Destination port` of the first packet (packet numbers start at 0)
 - the feedback that the teacher wants to give to the students who
   provide an incorrect answer. This feedback is specific to each
   hidden field.

![https://inginious.org/course/cnp3/tcp-syn-port]({{ site.baseurl
}}/images/inginious-tcp-5.png)

Inginious also allows the teacher to write exercises where the student
needs to reorder packets, possibly with some hidden fields. A first
example is
[https://inginious.org/course/cnp3/tcp-reorder-twh](https://inginious.org/course/cnp3/tcp-reorder-twh)
where the students have to reorder the TCP segments exchanged during
the three-way handshake

![https://inginious.org/course/cnp3/tcp-syn-port]({{ site.baseurl
}}/images/inginious-tcp-6.png)

More than a dozen of exercises are available from
[https://inginious.org/course/cnp3](https://inginious.org/course/cnp3). The
source code for all these exercises is available from
[https://github.com/obonaventure/CNP3-Inginious](https://github.com/obonaventure/CNP3-Inginious). Comments,
suggestions and ideas for new packet exercises are welcome as
[issues](https://github.com/obonaventure/CNP3-Inginious/issues) on github. 


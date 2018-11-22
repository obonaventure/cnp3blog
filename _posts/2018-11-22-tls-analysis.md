---
layout: post
title: A first analysis of a TLS server
tag: tls, firefox, wireshark, openssl
author: Olivier Bonaventure
---

Networking students can learn a lot about Internet protocols by analyzing how they are actually deployed. For several years, Computer Science students at [UCLouvain](https://www.uclouvain.be) have analyzed different websites within their introductory networking course. This project considers several key Internet protocols, DNS, HTTP, TLS and TCP. In this post, we briefly analyze how TLS is used on some websites as a starting point for these students.

The simplest way to analyze how a web server uses TLS is to start with the developer extensions that are included in most web browsers. In this post, we use [firefox](https://www.mozilla.org/en-US/firefox/new/) a very popular and open-source web browser.

Let us first start by looking at the [https://www.uclouvain.be](https://www.uclouvain.be) web site. This web site only uses two different domain names and does not include trackers and other third parties websites. Our first check is whether the web site is reachable without TLS. For this, we try to contact [http://www.uclouvain.be](http://www.uclouvain.be). A few years ago, the UCLouvain web site, like most web sites, was only reachable over HTTP and did not support HTTPS, i.e. HTTP over TLS.

![http://www.uclouvain.be]({{ site.baseurl }}/images/tls-1.png)


We observe that the web site is reachable over HTTP, but it immediately return an HTTP redirect that points to the HTTPS web site. A closer look at the security elements of one of the HTTPS request provides more information about the utilisation of TLS.


![https://www.uclouvain.be]({{ site.baseurl }}/images/tls-2.png)

First, the web site supports version 1.2 of TLS. Second, the cipher suite that was selected by the server for our version of Firefox is TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256. Some web sites provide information on best current practices in configuring the TLS cipher suites. One example is [https://github.com/ssllabs/research/wiki/SSL-and-TLS-Deployment-Best-Practices](https://github.com/ssllabs/research/wiki/SSL-and-TLS-Deployment-Best-Practices). A detailed knowledge of these cipher suites would be required to compare them in details. Some specialised web sites allow you provide detailed analysis of remote web sites. A good example is SSLLabs whose report for UCLouvain is available at [https://www.ssllabs.com/ssltest/analyze.html?d=www.uclouvain.be&latest](https://www.ssllabs.com/ssltest/analyze.html?d=www.uclouvain.be&latest)

We observe that the certificat was issued for uclouvain.be. There are two other elements that are interesting to analyse. The first one is the support for HTTP Strict Transport Security (HSTS) [RFC6797](https://tools.ietf.org/html/rfc6797). HSTS is a web security mechanism that enables web sites to indicate that they are only reachable over HTTPs. This helps to prevent attacks where attackers would remove the `S` for security from `https` in URLs found in web pages. Such attacks have been used in some countries to track the websites visited by some users. With HSTS, the web site returns in the HTTP responses a header that indicates that it should only be reached over HTTPS. This information is then cached by the browser.

![https://www.uclouvain.be]({{ site.baseurl }}/images/tls-3.png)

It is also possible to test a TLS server by using tools such as [openssl](https://www.openssl.org) on the command line. Several examples are provide in [Chapter 2](https://www.feistyduck.com/library/openssl-cookbook/online/ch-testing-with-openssl.html) of the [OpenSSL Cookbook](https://www.feistyduck.com/library/openssl-cookbook/).

The basic usage of openssl's s_client is shown below:

```console
openssl s_client -connect www.uclouvain.be:443
CONNECTED(00000003)
depth=2 C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Assured ID Root CA
verify return:1
depth=1 C = NL, ST = Noord-Holland, L = Amsterdam, O = TERENA, CN = TERENA SSL CA 3
verify return:1
depth=0 C = BE, ST = Ottignies-Louvain-la-Neuve, L = Louvain-la-Neuve, O = Universit\C3\A9 catholique de Louvain, OU = SGSI, CN = www.uclouvain.be
verify return:1
---
Certificate chain
 0 s:/C=BE/ST=Ottignies-Louvain-la-Neuve/L=Louvain-la-Neuve/O=Universit\xC3\xA9 catholique de Louvain/OU=SGSI/CN=www.uclouvain.be
   i:/C=NL/ST=Noord-Holland/L=Amsterdam/O=TERENA/CN=TERENA SSL CA 3
 1 s:/C=NL/ST=Noord-Holland/L=Amsterdam/O=TERENA/CN=TERENA SSL CA 3
   i:/C=US/O=DigiCert Inc/OU=www.digicert.com/CN=DigiCert Assured ID Root CA
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIF+jCCBOKgAwIBAgIQCt2ldEgid0xmxR3+YctiNDANBgkqhkiG9w0BAQsFADBk
MQswCQYDVQQGEwJOTDEWMBQGA1UECBMNTm9vcmQtSG9sbGFuZDESMBAGA1UEBxMJ
QW1zdGVyZGFtMQ8wDQYDVQQKEwZURVJFTkExGDAWBgNVBAMTD1RFUkVOQSBTU0wg
Q0EgMzAeFw0xNzAzMDEwMDAwMDBaFw0yMDAzMDUxMjAwMDBaMIGjMQswCQYDVQQG
EwJCRTEjMCEGA1UECBMaT3R0aWduaWVzLUxvdXZhaW4tbGEtTmV1dmUxGTAXBgNV
BAcTEExvdXZhaW4tbGEtTmV1dmUxKjAoBgNVBAoMIVVuaXZlcnNpdMOpIGNhdGhv
bGlxdWUgZGUgTG91dmFpbjENMAsGA1UECxMEU0dTSTEZMBcGA1UEAxMQd3d3LnVj
bG91dmFpbi5iZTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOUVUmjt
ZW/f8/xh27QopLf54xE2adr8fOKn91PpQfpnWC/7yP9X/kSnS3Tn8xsUqxxlVstA
2peJZFoZ6gFaER2XM42HmPp+UeYT+i5zLdQ1jMJUhEb6+PEbkkL1nDoP0H2XF88+
0PtARXAoT+MukAlMOxPo6cXL6GECzFn+0FEy8gZJsAyFqhUS8iAkVKPg+Yksm5NT
UUyeu38WhOfmIe943apjMnobjHX+r0ijOobypM2prDvAZvKlAx6hpnLlncLNnnO/
zF/y7fHur4Mrdutr5WWhUuZWB4DfaiRd2PcP3DJ8BoxkPaHxsZvvlky2ctlPHh0U
U27YU/+5KGmEXdECAwEAAaOCAmYwggJiMB8GA1UdIwQYMBaAFGf9iCAUJ5jHCdIl
GbvpURFjdVBiMB0GA1UdDgQWBBTVUIv5jhpiJyYtMdBl+MYch61khDCBtwYDVR0R
BIGvMIGsghB3d3cudWNsb3V2YWluLmJlggx1Y2xvdXZhaW4uYmWCFWludHJhbmV0
LnVjbG91dmFpbi5iZYIPY20udWNsb3V2YWluLmJlghZwb3J0YWlsLnNpcHIudWNs
LmFjLmJlgh9wb3J0YWlsLWludHJhbmV0LnNpcHIudWNsLmFjLmJlghpwb3J0YWls
LWNtcy5zaXByLnVjbC5hYy5iZYINd3d3LnVjbC5hYy5iZTAOBgNVHQ8BAf8EBAMC
BaAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMCMGsGA1UdHwRkMGIwL6At
oCuGKWh0dHA6Ly9jcmwzLmRpZ2ljZXJ0LmNvbS9URVJFTkFTU0xDQTMuY3JsMC+g
LaArhilodHRwOi8vY3JsNC5kaWdpY2VydC5jb20vVEVSRU5BU1NMQ0EzLmNybDBM
BgNVHSAERTBDMDcGCWCGSAGG/WwBATAqMCgGCCsGAQUFBwIBFhxodHRwczovL3d3
dy5kaWdpY2VydC5jb20vQ1BTMAgGBmeBDAECAjBuBggrBgEFBQcBAQRiMGAwJAYI
KwYBBQUHMAGGGGh0dHA6Ly9vY3NwLmRpZ2ljZXJ0LmNvbTA4BggrBgEFBQcwAoYs
aHR0cDovL2NhY2VydHMuZGlnaWNlcnQuY29tL1RFUkVOQVNTTENBMy5jcnQwDAYD
VR0TAQH/BAIwADANBgkqhkiG9w0BAQsFAAOCAQEAHtotGX8txgUsHibONJnor7ZW
IoDqQ/I+C4nTY0IaWAn/8Ob/t2DaOZa/FK2zfmiL++xyvhpGZfInl6poCHujN+Ka
i98n2X1Y4dFpf852S+mhbfi12RgtzQCt+R4NZIimPXmaPWcMFyc0St+ilhweGlg0
KxYm5IaJjz2xW7yF3HNl2/HDnQw0cZFmNp0gHmCUCtb0Ml3GjyHCFFuU0Hxx9hrE
qAa3KJaxJvBOh37YOb7PGYl0MlW96KE2IOQtziC+mkTEpCAMxYSzKp4cfHgUvDVu
gFG1TWWzFL1BtSFaPU6v/QHBRK4VssjGzjOJaAlptdn90dm/9a3lmp2Kvr/YnQ==
-----END CERTIFICATE-----
subject=/C=BE/ST=Ottignies-Louvain-la-Neuve/L=Louvain-la-Neuve/O=Universit\xC3\xA9 catholique de Louvain/OU=SGSI/CN=www.uclouvain.be
issuer=/C=NL/ST=Noord-Holland/L=Amsterdam/O=TERENA/CN=TERENA SSL CA 3
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 3328 bytes and written 434 bytes
---
New, TLSv1/SSLv3, Cipher is ECDHE-RSA-AES128-GCM-SHA256
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES128-GCM-SHA256
    Session-ID: 9770A205B374C2F1AF20B0A85022FD2A79C175C0AEBB245245D3B7598BBF3F0A
    Session-ID-ctx:
    Master-Key: 39C9D7F85E8F7AE7784D03B930C57AC28B2C3AD8315466BD762A34080FA1A4209D443EC3064947694B0EA328B0EE2896
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1542916368
    Timeout   : 300 (sec)
    Verify return code: 0 (ok)
---
closed
```

This output provides the TLS certificate and the outcome of the TLS negotiation. For some servers that utilise Server Name Indication, it might be necessary to provide it with the `-servername` option followed by the server name. It is also possible to force the utilisation of a specific version of TLS (the most widely deployed one today is 1.2 with some large servers that already support 1.3, but some servers might still support older versions that are less secure).

It is also interesting to see whether the server caches the negotiated keys for some time to allow clients to reconnect without having to perform again a secure handshake.

```console
openssl s_client -connect www.uclouvain.be:443 -reconnect
```

Another sources of information is to capture the packets that are exchanged with the server by using [tcpdump](https://www.tcpdump.org) or [WireShark](https://www.wireshark.org).  [tcpdump](https://www.tcpdump.org) works on the command line and can be used to store packets in a file for offline analysis, e.g.

```console
sudo tcpdump -s 1500 -w tls.pcap tcp port 443
```
captures the TCP packets on port 443 in file `tls.pcap` and limits the packet size to 1500 bytes.

Note that while capturing packets with [tcpdump](https://www.tcpdump.org) or [WireShark](https://www.wireshark.org), remember that these tools capture all the packets that are exchanged over the local network. If you use other applications than your browser (or background services), you might observe those packets in your trace.

As an example, here is the Server Hello sent by `www.uclouvain.be`. Note that the ClientHello is sent by your browser and thus does not reveal any information about the server.

![https://www.uclouvain.be]({{ site.baseurl }}/images/tls-4.png)


Here we se the session identifier, which is 32 bytes long, the cipher suite, the compression method (the absence of compression is a good sign as there have been attacks in the past on the compression methods that were used with TLS), the server name, ...

In this example, we leveraged the ability of Firefox to export the TLS keys so that they can be reused by Wireshark. This is explained on
[https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) and on the Wireshark [wiki](https://wiki.wireshark.org/SSL).

In a nutshell, to enable Wireshark to decrypt the captured TLS packets, you need to first launch Firefox with the `SSLKEYLOGFILE` environment variable pointing to a file. On the Mac, this corresponds to the following command. 

```
SSLKEYLOGFILE=~/tmp/log open /Applications/Firefox.app/
```

On Linux, you would use `SSLKEYLOGFILE=~/tmp/log firefox`. Google Chrome also supports this feature.

Then you need to tell to Wireshark to use this log file by clicking on Preferences->Protocols->SSL and provide the name of the file containing the keys.

![https://www.uclouvain.be]({{ site.baseurl }}/images/tls-5.png)

This can be also very useful to analyse how a server used HTTP/1.1 or HTTP/2.0 based on a packet trace.

These tools constitute a first starting point to explore the TLS configuration of a server. 


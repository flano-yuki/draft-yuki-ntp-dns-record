---
title: "NTP DNS Resource Record"
category: info

docname: draft-yuki-ntp-dns-record-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Internet"
workgroup: "Network Time Protocols"
keyword: internet-draft

author:
 -
    fullname:
      :: 後藤ゆき
      ascii: Yuki Goto
    organization: independent
    email: minami.hiroy@gmail.com

normative:

informative:

...

--- abstract

This document defines a new NTP DNS Resource Record, similar in concept to the HTTPS DNS Resource Record specified in RFC 9460.

This record enables an NTP server to indicate, via DNS, the versions ofthe NTP protocol it supports prior to the initiation of any NTP message exchange.

--- middle

# Introduction

NTPv5 is currently under standardization, and there are concerns regarding how clients select the newer NTP protocol version to use.

The server will drop NTP packets with unsupported versions. Even if a server implements support for NTPv5, a client will ordinarily initiate communication using NTPv4, assuming that NTPv4 is supported by the server. The server then advertises its NTPv5 support to the client using the NTPv4 reference timestamp.

The version of NTP used in the first request is therefore effectively based on implicit assumptions rather than explicit information, and the client has no reliable mechanism to determine the server’s supported versions in advance. This creates challenges for the deployment of future NTP protocol versions.

To address this challenge, this document defines a DNS-based mechanism similar to the HTTPS Resource Record (RFC 9460). This mechanism enables a server to advertise the NTP protocol versions it supports before a client initiates any NTP communication.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# NTP Resource Record
The NTP Resource Record (NTP RR) is a SVCB-compatible RR type, as defined in RFC 9460.
It uses the same RDATA wire format as the SVCB RR, but its semantics are specialized for discovery and configuration of Network Time Protocol (NTP) services.

## example
The following example illustrates the presentation format of the NTP RR, showing how a server advertises support for multiple NTP protocol versions using the ntp-version SvcParamKey.

~~~
ntp.example.com. 300 IN NTP 1 . ntp-version=4,5
~~~

## ntp-version SvcParams
The ntp-version SvcParamKey indicates the set of NTP protocol versions supported by the service endpoint. Its value is a comma-separated list of ASCII version identifiers. Each version identifier consists of a numeric version number, optionally followed by a hyphen and an alphanumeric label (e.g., “5-draft5”), allowing servers to indicate support for development, experimental, or otherwise distinguished variants of a protocol version.

ABNF:

~~~
version           = 1*DIGIT *( "-" version-label )
version-label     = 1*( ALPHA / DIGIT )
ntp-version-value = version *( "," version )
~~~

The wire-format consists of one or more version identifiers, each encodedas a length-prefixed byte sequence. These length–value pairs areconcatenated to form the SvcParamValue.

## nts SvcParams
( Is it useful to define nts SvcParams to indicate NTS support? )


# Security Considerations

TODO


# IANA Considerations

TODO


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

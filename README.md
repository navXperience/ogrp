OGRP
====

The Open Gnss Receiver Protocol

Introduction
============

Previous attempts to define an exchange protocol for GNSS related data
have been proposed only by manufacturers for their own products with
proprietary wire protocols or for limited applications, e.g. RTCM,
RINEX, NMEA.

OGRP (<b>O</b>pen  <b>G</b>NSS <b>R</b>eceiver <b>P</b>rotocol) tries to be a
protocol for many different causes in the domain of GNSS may it be
output of raw measurements from receivers over a physical wire
connection but also supporting RPC (remote procedure call) like
JSON-RPC.

OGRP is a protocol based on JSON ([RFC4627]).
and JSON Schema ( http://json-schema.org/documentation.html )

The transport mechanism is not part of the OGRP definition.

JSON just define sinlge Objects. But for a continous stream of data, for example 
via plain TCP socket, a file, a serial line with a
wrapping wire protocol or similar ways it ist necassary to define the "listing" of Objcts.
As first attempt we use a line delimiter.  [NDJ](See also: http://en.wikipedia.org/wiki/Line_Delimited_JSON).
Files containing OGRP should have the filename extension *.ogrp*.

OGRP is meant to be extendable and future-proof rather than optimized
regarding performance for a specific case. The main messages are defined and some of them can be 
extended by own properties.
Messages that are not defined can easily defined on your own fitting into the common scheme.
OGRP should also support complete wrapping of other message protocols in
itself, e.g. any message used in proprietary protocols should be expressable
by OGRP or embeddable in OGRP messages.
Because JSON is a Textformat it is not possible to fill in non-ascii codes. And the Data is always descriped and Escaped as a single String.
So there will always be some Problems with wrapping of other Messages. We need a special escape format for this
This is not implemented now, but will be considered in future.

To get an optimized performance we plan to develop a binary compressed represantation of the OGRP Data.
Therefore we will decripe a gerneral process for compressing and decoding.

For the predefined Messagetypes and Messageparts we use JSON-Schema draft v4 as a format for describing the Porperties.
With such definitions it is easily possible to check own or given data on OGRP comformance.


Conventions used
----------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in [RFC2119].

The underlying format used for OGRP is JSON. Consequently, terms and
types are to be interpreted as described in section 1 of [RFC4627].


Maturity
--------

Maturity indicator: Is this an experimental, draft, stable, legacy, or
retired specification?

A: OGRP is considered in draft state.

Extending the protocol / specification
--------------------------------------

Please always extend the given schema file or create an own schema file.
Create a testset of Data and control the schema file with a validator-tool.

Consider the following:

-   Use formal grammar: prevents arguments due to different
    interpretations of the text.

-   Use technical explanation: semantics of each message, error
    handling, and so on.

-   Add references: to other documents, protocols, and so on.

-   Use list-of-structs instead of struct-of-lists.

-   Use explicit and unambiguous key names.

-   Use lower-case, underscore separated key names.

-   You must use the "description" fields in the schema file for all dokumentation

-   Please think about all possible properties and define theme. 
    "type" and "description" is a must have.
    always think abaut "minimum", "required" and "additionalProperties" and declare them

Message specification
=====================

The Messages are specified by the JSON_Schema
For new Schemafiles consider this:

An OGRP message is composed of one JSON object. Two mandatory fields
exist

```
msg = {
    "protocol": protocol,
    "id": id,
    ...
}

protocol = "OGRP1"
id = string ; lower-case, underscore separated id, e.g. "measurement", ...
```

The additional content in the message is dependant on the message id.
Optional fields are allowed and has to be marked in the Schema.
Parser may ignore them if unknown
(according to JSON).



Key naming
----------

Names of keys should be explicit and unambiguous.
No alternative names are allowed:

Representing time and date
--------------------------

the general 'timestamp' holds the system_time of the sending tool. It is in seconds since 1.1.1970 0 OClock.
It can be a float value.

( to discuss )
()
Fields called 'timestamp' can hold times specified in UNIX time or ISO 8601. So if the JSON
type is float it's a UNIX time referred to UTC. If the JSON type is string, it must conform
to ISO 8601.
)

times based on GNSS Measurement are placed within the sprecial properties of the Message.


Usage
=====

Applications using OGRP for input or output should define the supported messages as [JSON Schema](http://json-schema.org/).
An example for a PVT application's minimum input data could be:

```JSON
{
	"channel_measurements": [
		{
			"carrier_phase": 0.5525,
			"channel_number": 3,
			"doppler": -2662.27,
			"gnss": "GPS",
			"locktime": 42.1,
			"pseudorange": 24985714.38,
			"satellite_id": 11,
			"signal_type": "L1CA",
			"snr": 46
		},
		{
			"carrier_phase": 0.783,
			"channel_number": 3,
			"doppler": -2662.245,
			"gnss": "GPS",
			"locktime": 16.8,
			"pseudorange": 24985714.38,
			"satellite_id": 11,
			"signal_type": "P1",
			"snr": 22.5
		},
		{
			"carrier_phase": 0.223,
			"channel_number": 3,
			"doppler": -2074.559,
			"gnss": "GPS",
			"locktime": 16.8,
			"pseudorange": 24985714.38,
			"satellite_id": 11,
			"signal_type": "P2",
			"snr": 25.2
		},
		{
			"carrier_phase": 3.919,
			"channel_number": 6,
			"doppler": -1826.736,
			"gnss": "GPS",
			"locktime": 36.9,
			"pseudorange": 21196204.06,
			"satellite_id": 22,
			"signal_type": "L1CA",
			"snr": 47.6
		},
		{
			"carrier_phase": 3.957,
			"channel_number": 6,
			"doppler": -1826.723,
			"gnss": "GPS",
			"locktime": 29.3,
			"pseudorange": 21196204.06,
			"satellite_id": 22,
			"signal_type": "P1",
			"snr": 29.8
		},
		{
			"carrier_phase": 3.3025,
			"channel_number": 6,
			"doppler": -1423.415,
			"gnss": "GPS",
			"locktime": 29.3,
			"pseudorange": 21196204.06,
			"satellite_id": 22,
			"signal_type": "P2",
			"snr": 29.7
		},
		{
			"carrier_phase": 2.683,
			"channel_number": 15,
			"doppler": -5232.769,
			"gnss": "GPS",
			"locktime": 12.8,
			"pseudorange": 21237850.66,
			"satellite_id": 5,
			"signal_type": "L1CA",
			"snr": 46.2
		},
		{
			"carrier_phase": 2.7205,
			"channel_number": 15,
			"doppler": -5232.796,
			"gnss": "GPS",
			"locktime": 5.4,
			"pseudorange": 21237850.66,
			"satellite_id": 5,
			"signal_type": "P1",
			"snr": 1
		},
		{
			"carrier_phase": 1.537,
			"channel_number": 15,
			"doppler": -4077.532,
			"gnss": "GPS",
			"locktime": 5.5,
			"pseudorange": 21237850.66,
			"satellite_id": 5,
			"signal_type": "P2",
			"snr": 1
		},
		{
			"carrier_phase": 0.6225000000000001,
			"channel_number": 16,
			"doppler": 693.033,
			"gnss": "GPS",
			"locktime": 42.3,
			"pseudorange": 24594600.66,
			"satellite_id": 15,
			"signal_type": "L1CA",
			"snr": 45.1
		},
		{
			"carrier_phase": 0.8545,
			"channel_number": 16,
			"doppler": 693.039,
			"gnss": "GPS",
			"locktime": 27.3,
			"pseudorange": 24594600.66,
			"satellite_id": 15,
			"signal_type": "P1",
			"snr": 26.4
		},
		{
			"carrier_phase": -1.7435,
			"channel_number": 16,
			"doppler": 540.058,
			"gnss": "GPS",
			"locktime": 27.3,
			"pseudorange": 24594600.66,
			"satellite_id": 15,
			"signal_type": "P2",
			"snr": 26.4
		},
		{
			"carrier_phase": -0.8025,
			"channel_number": 17,
			"doppler": -2338.205,
			"gnss": "GPS",
			"locktime": 40.7,
			"pseudorange": 24261454.92,
			"satellite_id": 17,
			"signal_type": "L1CA",
			"snr": 46.7
		},
		{
			"carrier_phase": -0.5695,
			"channel_number": 17,
			"doppler": -2338.16,
			"gnss": "GPS",
			"locktime": 13.4,
			"pseudorange": 24261454.92,
			"satellite_id": 17,
			"signal_type": "P1",
			"snr": 22.5
		},
		{
			"carrier_phase": -0.7455000000000001,
			"channel_number": 17,
			"doppler": -1822.01,
			"gnss": "GPS",
			"locktime": 13.4,
			"pseudorange": 24261454.92,
			"satellite_id": 17,
			"signal_type": "P2",
			"snr": 22.5
		},
		{
			"carrier_phase": 3.2465,
			"channel_number": 18,
			"doppler": -5580.635,
			"gnss": "GPS",
			"locktime": 36.8,
			"pseudorange": 21905838.34,
			"satellite_id": 14,
			"signal_type": "L1CA",
			"snr": 46.4
		},
		{
			"carrier_phase": 3.4745,
			"channel_number": 18,
			"doppler": -5580.595,
			"gnss": "GPS",
			"locktime": 14.7,
			"pseudorange": 21905838.34,
			"satellite_id": 14,
			"signal_type": "P1",
			"snr": 25.4
		},
		{
			"carrier_phase": 1.1925,
			"channel_number": 18,
			"doppler": -4348.517,
			"gnss": "GPS",
			"locktime": 14.7,
			"pseudorange": 21905838.34,
			"satellite_id": 14,
			"signal_type": "P2",
			"snr": 27.2
		},
		{
			"carrier_phase": 3.6585,
			"channel_number": 19,
			"doppler": -1115.432,
			"gnss": "GPS",
			"locktime": 45.40000000000001,
			"pseudorange": 20781607.1,
			"satellite_id": 9,
			"signal_type": "L1CA",
			"snr": 47.7
		},
		{
			"carrier_phase": 3.8865,
			"channel_number": 19,
			"doppler": -1115.414,
			"gnss": "GPS",
			"locktime": 27.3,
			"pseudorange": 20781607.1,
			"satellite_id": 9,
			"signal_type": "P1",
			"snr": 29.5
		},
		{
			"carrier_phase": 1.5485,
			"channel_number": 19,
			"doppler": -869.1610000000001,
			"gnss": "GPS",
			"locktime": 27.3,
			"pseudorange": 20781607.1,
			"satellite_id": 9,
			"signal_type": "P2",
			"snr": 28.4
		}
	],
	"id": "channel_measurements",
	"protocol": "OGRP1",
	"timestamp": 1420066248
}
```
Security considerations
=======================

See JSON considerations on this topic in [RFC4627].

No further security mechanisms are designed into OGRP.


References
==========

 * [RFC2119]
[RFC2119]: <http://www.ietf.org/rfc/rfc2119.txt> "RFC2119"
 * [RFC4627]
[RFC4627]: <http://www.ietf.org/rfc/rfc4627.txt> "RFC4627"


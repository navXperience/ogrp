#OGRP
<b>Open Gnss Receiver Protocol for Open Source Receiver</b>

Background 
----------
During the attempt to develop an open source receiver we needed a protocol for exchanging data between the components of a GNSS Receiver or in connection with external Hard or Software. The protocols which were available when we start were optimized for special cases or products, they were not open nor complete.
So we decided to create a new one that may solve in almost any situation we/you need to transport data.

Preconditions
-------------
OGRP should be easy to learn, easy to read and easy to handle. It should be extendible by own needs and it should cover standard cases unambiguous. 

early decisions
---------------
We decided to use JSON as underlying format. To get rid of ambiguous interpretation we decided to use 
JSON Schema ( http://json-schema.org/documentation.html ) as definition format. 

The transport mechanism of data is not part of the OGRP definition. But the definition allows transportation with all common mechanisms as TCP/IP, Socket or File.

details of the format
---------------------

JSON just define single Objects. For a continuous stream of data, for example 
via plain TCP socket, a file, a serial line with a
wrapping wire protocol or similar ways you just send the complete objects separated by a new line.

The main messages are defined and some of them can be extended by own properties.
For the predefined message types and message parts we use JSON-Schema draft v4 as a format to describe the properties.
With such definitions it is easy to check own or given data on OGRP conformance.
Messages which are not defined can easily defined on your own fitting into the common scheme.
Please create your own JSON-Schema file to make the extension usable by other groups.

future aspects
--------------
To get an optimized performance we plan to develop a binary compressed representation of the OGRP data.
Therefore we will describe a general process for compressing and decoding.

State of development
--------------------
Currently we are working on the first official version. Till then the schema is not stable.

Create own extensions
---------------------

Please always extend the given schema file or create an own schema file.
[JSON Schema](http://json-schema.org/).

Create a test set of data and control the schema file with a validator-tool.

When creating or extending follow these rules:

-   Each message type which is used as a single OGRP object has at least 3 fields which are always needed.
    - "id"  containing the unique messagetypename
    - "version" in Version 1 this is always OGRP1
    - "timestamp" that contains the time of sending the message

-   For building names and declarations try to use common phrases, try to reuse phrases. 

-   Try to get short names and declarations but they has to be clear and readable.
    Details should be described in the "description" part.

-   You must use the "description" fields in the schema file for all documentation.

-   Please think about all possible properties and define theme. 
    "type" and "description" is a must have.
    Always think about "minimum", "required" and "additionalProperties" and declare them.

Usage
-----
To build or parse an OGRP Message you depend on the message type which ist declared in the "id". Depending on that you has to fill or read the rest of the object.  
Optional fields are allowed and has to be marked in the schema.
Writer don’t must fill optional fields.

The general 'timestamp' holds the system time of the sending tool. It is in seconds since 1.1.1970 0 O’clock.
It can be a float value. Please have a look at the Schemafiles.

Times based on GNSS Measurement are placed within the special properties of the message.

Example
-------
You find examples in the folder "Examples"

For a first look, here is an example for a PVT applications minimum input data:

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

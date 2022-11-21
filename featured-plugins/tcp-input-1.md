---
description: Enables the data ingestion through a TCP input port
---

# TCP Input

The TCP input plugin is available for download on [Intelie Live Marketplace](https://marketplace.intelie.com/artifact/plugin-tcpinput).

![Example of basic TCP Input configuration](<../.gitbook/assets/image (28).png>)

A brief explanation of each parameter:

* Input port: the TCP port on which it will listen for connections.
* type field in JSON: when "allow type to be defined in the event" is checked, the field which will have the event type, by default "\_\_type".
* Default type: the event type to be used if "allow type to be defined in the event" is unchecked, or if the event does not have the field with the event type.
* Max connections (min version: `2.26.3`)
* Max connections queue size (min version: `2.26.3`)
* Allow type to be defined in the event: when checked, use the field defined as the "type field in JSON" to define the event type.
* Skip real-time: do not send events received in this connection to real-time queries.

![Allowed/Denied addresses list](<../.gitbook/assets/image (67).png>)

Each of the "allowed addresses" and "denied addresses" fields are a space-separated list of CIDR ranges, for instance, "192.0.2.0/24 198.51.100.0/24 2001:db8::/32". The "deny addresses by default" checkbox controls the order in which the IP address of the connecting peer is checked against these two lists.

If "deny addresses by default" is not checked, a connection is allowed if the IP address of the peer is in the "allowed addresses" list; otherwise, the connection is denied if it is in the "denied addresses" list; otherwise, the connection is allowed.

If "deny addresses by default" is checked, a connection is denied if the IP address of the peer is in the "denied addresses" list; otherwise, the connection is allowed if it is in the "allowed addresses" list; otherwise, the connection is denied.



Let's use this configuration as an example:&#x20;

Allowed addresses:  "192.0.2.0/24 198.51.100.3"

Denied addresses: "192.0.2.10 198.51.100.0/24"

| IP           | deny addresses by default is not checked | deny addresses by default is checked |
| ------------ | ---------------------------------------- | ------------------------------------ |
| 192.0.2.10   | ✔️ allowed                               | 🚫 denied                            |
| 192.0.2.42   | ✔️ allowed                               | ✔️ allowed                           |
| 198.51.100.3 | ✔️ allowed                               | 🚫 denied                            |
| 198.51.100.2 | 🚫 denied                                | 🚫 denied                            |
| 203.0.113.1  | ✔️ allowed                               | 🚫 denied                            |


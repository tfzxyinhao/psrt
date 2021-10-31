# PubSubRT - an industrial pub/sub server

## What is PSRT

PSRT is pub-sub protocol, optimized for industrial needs: low latency, slow
channels and large payloads.

## Why not MQTT?

We love MQTT. And we use MQTT a lot. There are cases where MQTT ideally fits
requirements. However, for some it does not satisfy our speed and reliability
needs and produces additional overhead. That is why we invented PSRT.

## What is the difference?

* PSRT is the protocol, optimized for large (65K+) messages
* No QoS - all messages are always delivered to subscribers only once, so
  consider it has QoS=2 if using MQTT measurements
* No retain topics. Retains usually require disk writes, which produce
  additional overhead
* No OP-ACK loops. All control operations are fast, synchronous and atomic
* Two TCP sockets: one for control ops and the second one for incoming
  messages. This makes clients a bit more complicated, but allows to process
  incoming messages without additional overhead. Additionally, with two sockets
  control op acknowledgements and incoming message data can be mixed, which is
  important when large messages are processed on slow channels
* Devices and nodes, which do not need subscriptions, can use either a single
  TCP control socket or work without connection established, using UDP
  datagrams with or without acknowledge from the server
* PSRT is almost 100% logically compatible with MQTT, so software can be
  switched to it and vice-versa with only a couple of lines of code

## About the repository

This repository contains PubSubRT server, implementing PSRT (open-source
version without the cluster module), command-line client and Rust client
library.

Other repositories:

* [psrt-py](https://github.com/alttch/psrt-py) - Python client library (sync)

## Example configuration files

If installed manually, get configuration files from
<https://github.com/alttch/psrt/tree/main/make-deb/etc/psrtd> or use files from
*example-configs* directory (need to be edited before use).

## Authentication

PubSubRT uses the standard *htpasswd* format, use any htpasswd-compatible tool
(with -B flag for bcrypt).

## Cargo crate

<https://crates.io/crates/psrt>

## Protocol specifications

<https://github.com/alttch/psrt/blob/main/proto.md>
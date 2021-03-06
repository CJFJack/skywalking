# Protocols
There are two types of protocols list here. 

- [**Probe Protocol**](#probe-protocols). Include the descriptions and definitions about how agent send collected metrics data and traces, also the formats of each entities.

- [**Query Protocol**](#query-protocol). The backend provide query capability to SkyWalking own UI and others. These queries are based on GraphQL.


## Probe Protocols
They also related to the probe group, for understand that, look [Concepts and Designs](../concepts-and-designs/README.md) document.
These groups are **Language based native agent protocol**, **Service Mesh protocol** and **3rd-party instrument protocol**.

### Register Protocol
Register protocol is shared in `Language based native agent protocol`[Required] and `Service Mesh probe protocol`[Optional]. 
Service, Service Instance, Network address and Endpoint could do register to get an unique integer ID to replace the literal string name and attributes. The probe could use IDs to reduce the cost of memory and network bandwidth. Service and Instance register are required in the language agent. All other cases, register are optional.

### Language based native agent protocol
There is two types of protocols to make language agents work in distributed environments.
1. **Cross Process Propagation Headers Protocol** and **Cross Process Correlation Headers Protocol** are in wire data format, 
agent/SDK usually uses HTTP/MQ/HTTP2 headers
to carry the data with rpc request. The remote agent will receive this in the request handler, and bind the context 
with this specific request.
1. **Trace Data Protocol** is out of wire data, agent/SDK uses this to send traces and metrics to skywalking or other
compatible backend. 

[Cross Process Propagation Headers Protocol v3](Skywalking-Cross-Process-Propagation-Headers-Protocol-v3.md) is the new protocol for 
in-wire context propagation, started in 8.0.0 release.

[Cross Process Correlation Headers Protocol v1](Skywalking-Cross-Process-Correlation-Headers-Protocol-v1.md) is a new in-wire context propagation additional and optional protocols. 
Please read SkyWalking language agents documentations to see whether it is supported. 
This protocol defines the data format of transporting custom data with `Cross Process Propagation Headers Protocol v2`.
SkyWalking javaagent begins to support this since 8.0.0.

[SkyWalking Trace Data Protocol v3](Trace-Data-Protocol-v3.md) defines the communication way and format between agent and backend.


### Service Mesh probe protocol
The probe in sidecar or proxy could use this protocol to send data to backendEnd. This service provided by gRPC, requires 
the following key info:

1. Service Name or ID at both sides.
1. Service Instance Name or ID at both sides.
1. Endpoint. URI in HTTP, service method full signature in gRPC.
1. Latency. In milliseconds.
1. Response code in HTTP
1. Status. Success or fail.
1. Protocol. HTTP, gRPC
1. DetectPoint. In Service Mesh sidecar, `client` or `server`. In normal L7 proxy, value is `proxy`.


### 3rd-party instrument protocol
3rd-party instrument protocols are not defined by SkyWalking. They are just protocols/formats, which SkyWalking is compatible and
could receive from their existed libraries. SkyWalking starts with supporting Zipkin v1, v2 data formats.

Backend is based on modularization principle, so very easy to extend a new receiver to support new protocol/format.

## Query Protocol
Query protocol follows GraphQL grammar, provides data query capabilities, which depends on your analysis metrics.
Read [query protocol doc](query-protocol.md) for more details.

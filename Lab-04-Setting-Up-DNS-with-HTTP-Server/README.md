# Cisco Packet Tracer Lab: DNS and HTTP Routing

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet_Tracer-blue?logo=cisco)
![Networking](https://img.shields.io/badge/Networking-DNS_%7C_HTTP_%7C_Routing-success)

## Overview
This laboratory demonstrates the configuration, routing, and operation of Domain Name System (DNS) and HTTP services across a multi-subnet network using Cisco Packet Tracer. It provides a visual trace of the packet flow during DNS resolution and subsequent HTTP requests.

## Network Topology
The network architecture is divided into three distinct subnets, routed via a central Cisco 2911 series router:
* **Client LAN:** `1.0.0.0/8`
* **HTTP Server LAN:** `2.0.0.0/8`
* **DNS Server LAN:** `3.0.0.0/8`

![Network Topology](/images/DNS_Configuration_github.png)

## Configuration Breakdown

### 1. Router Interfaces
The central router (`Router0`) provides inter-VLAN/inter-subnet routing. The GigabitEthernet interfaces are configured with the default gateway IP addresses for their respective networks.

![Router Configuration](/images/DNS_Configuration_IOS_command.png)

### 2. DNS Server Configuration
The DNS Server (IP: `3.0.0.2`) is configured to resolve the fully qualified domain name (FQDN). An `A Record` maps `upit.ro` to the HTTP Server's IPv4 address (`2.0.0.2`).

![DNS A Record Configuration](/images/DNS_record.png)

### 3. Client Provisioning
The client machines (e.g., `PC0`) are statically assigned IP addresses along with the correct default gateway and DNS server address to allow external resolution.

![Client IP and DNS Configuration](/images/DNS_Configuration_dns_setting.png)

## Simulation & Verification
The demonstration below captures the simulation panel in Packet Tracer. It verifies the end-to-end connectivity by initiating a web browser request from `PC0` to `http://upit.ro`. 

The trace shows:
1. The outbound DNS query routed to `3.0.0.2`.
2. The DNS response returning the `2.0.0.2` address.
3. The HTTP GET request routed to the web server, successfully rendering the University portal.

<video src="/images/dns.mp4" controls="controls" style="max-width: 100%;">
  Your browser does not support the video tag.
</video>


## Repository Contents
* `DNS.pkt`: The complete Cisco Packet Tracer simulation file.
* Configuration screenshots and simulation recording.

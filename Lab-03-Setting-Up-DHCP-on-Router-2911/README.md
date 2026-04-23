# Laboratory Report: Comprehensive Configuration of Dynamic Host Configuration Protocol (DHCP) on a Cisco 2911 Router

## 1. Objective
The primary objective of this laboratory exercise is to configure a Cisco 2911 Integrated Services Router (ISR) to function as a DHCP server. This configuration automates the distribution of IPv4 addresses, subnet masks, and default gateway parameters to end-user devices within a local area network, ensuring efficient address management and preventing manual configuration errors.

## 2. Network Topology
The network topology consists of a Cisco 2911 Router acting as the default gateway and DHCP server, connected via a Layer 2 switch to three client workstations.

![Figure 1: Network Topology Diagram](./images/ping_successful.png)

---

## 3. Router Configuration Procedure
The following configuration initializes the router, configures the primary gateway interface, and establishes the DHCP pool parameters in a single sequence.

```text
Router> enable
Router# configure terminal
Router(config)# hostname DHCP_R1
DHCP_R1(config)# interface g0/0
DHCP_R1(config-if)# ip address 192.168.0.1 255.255.255.0
DHCP_R1(config-if)# no shutdown
DHCP_R1(config-if)# exit
DHCP_R1(config)# ip dhcp excluded-address 192.168.0.1
DHCP_R1(config)# ip dhcp pool IP_POOL1
DHCP_R1(dhcp-config)# network 192.168.0.0 255.255.255.0
DHCP_R1(dhcp-config)# default-router 192.168.0.1
DHCP_R1(dhcp-config)# exit
DHCP_R1(config)# end
DHCP_R1# write memory
```

![Figure 2: Complete Router DHCP CLI Sequence](./images/router_dhcp_simple.png)

---

## 4. Client Device Configuration
To receive an automated address, the workstation NIC settings must be set to DHCP mode. This initiates the DORA process to lease parameters from the router.

* **Method:** DHCP
* **Target Subnet:** `192.168.0.0/24`
* **Default Gateway:** `192.168.0.1`

---

## 5. Verification and Connectivity Testing
Verification is conducted by confirming the IP lease on the client and testing Layer 3 reachability to the gateway.

### 5.1 DHCP Address Assignment
The client (PC0) successfully obtained an IP address of `192.168.0.2` from the `IP_POOL1` range.

![Figure 3: Successful DHCP Request on Client](./images/dhcp_request_succesfull.png)

### 5.2 ICMP Connectivity Test
A ping is executed from the client command prompt to the gateway interface to verify end-to-end connectivity.

```text
C:\> ping 192.168.0.1

Pinging 192.168.0.1 with 32 bytes of data:
Reply from 192.168.0.1: bytes=32 time<1ms TTL=255
Reply from 192.168.0.1: bytes=32 time<1ms TTL=255
Reply from 192.168.0.1: bytes=32 time<1ms TTL=255
Reply from 192.168.0.1: bytes=32 time=3ms TTL=255

Ping statistics for 192.168.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

---

## 6. Conclusion
The Cisco 2911 router was successfully configured as a DHCP server. By excluding the gateway address and defining the network scope, the router provides seamless IP management for the LAN. Connectivity was verified through successful ICMP replies, confirming the integrity of the automated addressing scheme.

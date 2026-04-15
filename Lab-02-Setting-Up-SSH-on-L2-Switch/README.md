# Laboratory Report: Comprehensive Configuration of Secure Shell (SSH) on a Layer 2 Managed Switch

## 1. Objective
The primary objective of this laboratory exercise is to configure a factory-default Layer 2 managed switch for secure remote management utilizing the Secure Shell (SSH) protocol. This procedure has been expanded to encompass enterprise-standard best practices, including establishing a management IP address, configuring a default gateway, generating RSA cryptographic keys, implementing a Message of the Day (MOTD) banner, establishing tiered user access control, and properly securing both console and virtual terminal (VTY) lines.

## 2. Network Topology
The network topology for this exercise consists of a single Layer 2 managed switch connected to localized end-user devices (workstations or laptops). 

![Figure 1: Network Topology Diagram](./images/topology_proof.png)

---

## 3. Switch Configuration Procedure

The following sections detail the step-by-step configuration required to secure the device.

### 3.1 Initial Setup and Global Security Parameters
The initial configuration assigns a hostname, secures the overarching Privileged EXEC mode, and establishes a warning banner for unauthorized users.

```text
Switch> enable
Switch# configure terminal
Switch(config)# hostname Sw1
Sw1(config)# enable secret cisco
Sw1(config)# banner motd #
*****************************************************************
* WARNING: Unauthorized access to this device is prohibited.    *
* All actions are monitored and logged. Disconnect immediately. *
*****************************************************************
#
```

### 3.2 Management Interface and Routing Configuration
The switch is assigned an IP address on the management Virtual Local Area Network (VLAN 1). A default gateway is also configured to allow SSH access from disparate networks.

```text
Sw1(config)# interface vlan 1
Sw1(config-if)# ip address 192.168.0.1 255.255.255.0
Sw1(config-if)# no shutdown
Sw1(config-if)# exit
Sw1(config)# ip default-gateway 192.168.0.254
```

### 3.3 Cryptographic Key Generation and Tiered User Access
A domain name is assigned prior to generating 2048-bit RSA keys. Two tiers of administrative accounts are created to demonstrate privilege separation.

```text
Sw1(config)# ip domain-name lab.local
Sw1(config)# crypto key generate rsa
! (When prompted for the modulus size, specify 2048)

Sw1(config)# username admin privilege 15 secret cisco
Sw1(config)# username tester privilege 1 secret cisco
```

### 3.4 Securing Access Lines and Saving Configuration
Both the physical console port and the virtual terminal lines are secured. The VTY lines are explicitly restricted to SSH.

```text
Sw1(config)# line console 0
Sw1(config-line)# logging synchronous
Sw1(config-line)# exec-timeout 5 0
Sw1(config-line)# exit

Sw1(config)# line vty 0 4
Sw1(config-line)# login local
Sw1(config-line)# transport input ssh
Sw1(config-line)# logging synchronous
Sw1(config-line)# exec-timeout 15 0
Sw1(config-line)# exit

Sw1(config)# end
Sw1# write memory
```

---

## 4. Client Device Configuration
To execute the connectivity test locally, an end-user device must be configured with a static IP address residing on the same logical subnet as the switch's management interface.

* **IPv4 Address:** `192.168.0.10`
* **Subnet Mask:** `255.255.255.0`
* **Default Gateway:** `192.168.0.254`

---

## 5. Verification and Connectivity Testing

Verification is conducted by initiating an SSH session from the client device's command-line interface. 

### 5.1 Testing Tier 1 Access (Direct Administrator)
```text
C:\> ssh -l admin 192.168.0.1
Password: [cisco]
Sw1#
```

### 5.2 Testing Tier 2 Access (Standard User to Enable Mode)
```text
C:\> ssh -l tester 192.168.0.1
Password: [cisco]
Sw1> enable
Password: [cisco]
Sw1#
```

![Figure 2: Successful SSH Authentication and Session Establishment](./images/ssh_success_proof.png)

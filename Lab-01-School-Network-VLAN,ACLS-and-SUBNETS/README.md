# Secure Corporate Network Infrastructure (Cisco Packet Tracer)

## Project Overview
This project simulates a secure, segmented corporate network environment. The primary objective was to design and configure a network topology that isolates department traffic, enforces strict access control policies, secures remote administrative access, and centralizes data storage and configuration backups.

### Technologies & Protocols Used:
* **Routing & Switching:** Cisco IOS, VLANs (802.1Q), Inter-VLAN Routing
* **Security:** Extended Access Control Lists (ACLs), SSHv2, RSA Encryption (2048-bit)
* **Services:** ICMP, FTP, TFTP

---

## 1. Layer 2 Segmentation & VLAN Configuration
To reduce the broadcast domain and logically isolate network traffic, access ports on the Cisco 2960 switches were assigned to dedicated VLANs. 

* **Action:** Created VLAN 2 and assigned FastEthernet ports `Fa0/1 - Fa0/5` as static access ports.
* **Proof of Configuration:**
![VLAN Configuration](images/vlan_switch_config.png)

---

## 2. Layer 3 Routing & Default Gateways
The Cisco ISR was configured to act as the default gateway for the segregated networks, allowing the router to manage and inspect traffic flowing between subnets.

* **Action:** Configured physical interfaces `Gig0/0` (192.168.10.1/24) and `Gig0/1` (192.168.20.1/24) and brought the line protocols up.
* **Proof of Configuration:**
![Router Interfaces](images/iso__commands_router_gig0-1.png)

---

## 3. Secure Remote Management (SSHv2)
Standard Telnet transmits credentials in plaintext, which is a severe security risk. Remote access to the core router was secured using SSH version 2.

* **Action:** Disabled Telnet on VTY lines 0-4. Generated a 2048-bit RSA crypto key, created a local administrative user, and configured a strict unauthorized access MOTD banner.
* **Proof of Configuration & Successful Connection:**
![SSH Configuration](images/ssh_enabled_router.png)
![Successful SSH Session](images/ssh_works_from_pc_to_router.png)

---

## 4. Network Isolation via Access Control Lists (ACLs)
Although the router natively routes traffic between connected subnets, security policy dictates that the `192.168.10.0/24` network must not communicate with the `192.168.20.0/24` network.

* **Action:** Deployed an Inbound Extended ACL (101/102) on the router's Gigabit interfaces to explicitly drop traffic destined for the opposing subnet while permitting all other outbound traffic.
* **Proof of Configuration & Packet Drop:**
![ACL Configuration](images/ip_access_lis_router.png)
*(Simulation panel verifying the router dropping an unauthorized ICMP packet based on ACL 101 rules):*
![ACL Packet Drop](images/ip_acl_screenshot.png)

---

## 5. Centralized Services: FTP & TFTP
A centralized Data Server was deployed to handle file storage and configuration backups. 

* **Action:** Verified end-to-end connectivity via ICMP. Configured the FTP service with secure credentials for user file storage, and utilized the TFTP service to backup the router's active configuration.
* **Proof of Connectivity & FTP File Transfer:**
![Server Ping Test](images/ping_from_server_to_router.png)
![FTP File Transfer](images/ftp_server.png)

* **Proof of TFTP Configuration Backup:**
![TFTP Server Status](images/tftp_R1_tftp.png)
![Router TFTP Backup Command](images/tftp_save_from_server_to_router.png)

# PFE


https://www.firewall.cx/networking/network-protocols/ipsec-modes.html

each *fortigate* or *router* will act as a *vpn gateway* for his LAN.

> **Tunneling** 
is a network technology that enables the 
encapsulation of one type of protocol packet within 
the datagram of a different protocol.

## IPSec

**1. Introduction to IPSec:**

Objective: Provide security services for IP packets, including encryption, authentication, protection against replay, and data confidentiality.
Protocols: Encapsulating Security Payload (ESP) and Authentication Header (AH).

### 2. IPSec Modes - Tunnel vs. Transport:

Tunnel Mode: Default mode where the entire original IP packet is protected by IPSec. Used between gateways or end-station to gateway.
Transport Mode: Used for end-to-end communications, protecting the IP payload. Common for client-server or workstation-gateway communication.

**3. IPSec Tunnel Mode:**

Function: Wraps, encrypts, and adds a new IP header to the original packet.
Use Cases: Between secure IPSec Gateways (e.g., Cisco routers) or VPN clients and gateways.
Header Placement: IPSec header (AH or ESP) inserted between the IP header and upper layer protocol.
Illustration: Packet diagrams provided for both ESP and AH headers in Tunnel mode.

**4. IPSec Transport Mode:**

Function: Protects the IP payload in end-to-end communication.
Use Cases: Encrypted sessions, e.g., Telnet or Remote Desktop, between a client and a server or workstation and a gateway.
Header Placement: IPSec headers encapsulate the IP payload; original IP headers remain intact.
Illustration: Packet diagrams for both ESP and AH headers in Transport mode.

**5. ESP vs. AH in Tunnel and Transport Modes:**

Tunnel Mode: ESP commonly used; AH may be used alone or with ESP.
Transport Mode: ESP commonly used; AH may be used alone or with ESP.
Header Identification: ESP identified with IP protocol ID 50, AH with ID 51.

**6. Conclusion:**

Choice of Mode: Depends on specific requirements and implementation of IPSec.
Application: Tunnel mode for secure gateways, transport mode for end-to-end communications.
This summary organizes key information from the provided article, distinguishing between IPSec Tunnel Mode and IPSec Transport Mode, and highlighting the role of ESP and AH in different scenarios.

## IKEv1
## IKEv2
## IKEv1 vs IKEv2
## Phase 1 
## Phase 2 
- Pre-shared keys are not used in encripting IKEv2 - only DH values are used
- Nuilt-in NAT-T support
- EAP support for authentication
- allow flexible auth choices (asymetrical)


---

Route-Based: 
    Fortigate automatically add a virtual interface with the VPN name
    Configure routing and firewall policies for IPSec Traffic the same way you do for non-IPSec traffic
    Leverage the presence of multiple connections to the same destination to achieve **redundancy** 
    you can enable dynamic routing protocols (= **Scalability**)
Policy-Based:
    Legacy, VPN matching based on policy, not recommended


## Firewall Policies for IPSec VPN

you must configure 2 policies, 
- allow inspect incoming traffic & outgoing traffic of the IPsec Virtual interface

### Redundant VPNs

- use 2 ISP on your site and deploy 2 IPSec VPNs, if the primary IPSec fails, the other can be used

![Alt text](image.png) 



## Goals de NG-IAP:

### Objectives principales du design:
- Remplacage des hardwares **EoL** ou/et **Non-Compliant**
- **Standarisation** d'infrastucture IAP
- **Secure Browsing** securisation du browsing d'internet pour aligner avec les normes de conformité de *Darwin2 security* (
  Deciphering,
  Anti-Malware,
  Anti-Virus,
  DLP Inspections
)

>Darwin-2 initiatives require deciphering browsing traffic to perform security inspections, such as antivirus, anti-malware and DLP.

### Autres objectives:

- **Remote Access**
- **Cloud Access**

- Virtualization d'une partie d'IAP pour:
  Simplifier le logistique, MCO, energie et cout.

- Utilisation de Cisco ENCS pour:
  - préparer pour le future dans levolution d'infrastructure
  - réduire les efforts de management(design, deploy, management des services réseaux à partir d'une seul interface(Cisco Enterprise
NFVIS GUI))
- Réduire les couts
  - Reduce physical hosting space (1 Rack Unit)
  - Reduce power and cooling consumption



## GTS IAP Architecture:

- SG possède plusieurs IAP pour différents besoins, qui sont tous conformes avec standard de sécurité (ANSSI)pour éviter une *compromise* 
- y a des *IAP de hosting* qui permet au utilisateurs externes d'acceder au applications et ressources internes
- y a des *IAP Hybrid* qui permet d'avoir un flux entrant et sortant, utilisé par les SSL VPN et la partie Browsing

### Browsing

le browsing dans SG est établie d'une maniere purement professionel pour garantire la sécurité des SI et des utilisateurs

## Solution Overview

- Upper filtering and Admin layer will rely on physical hardware
- Cisco ENCS technology will be introduced to virtualize lower filtering and services layer.

(Group requirement is to have Upper and Lower Firewalls on different hardware, that’s why we can’t
mutualize.)

### Silo VPN, Partners & Remote Access
VPN Ipsec between the partner and the VPN Services on the ENCS. All flow coming from partners will transit by a specific firewall to restrict the access to internal server by the partners. Physical Netscaler will permut to have protocol break between the two lines of firewalls.
- Partners Firewall
- VPN services IPSec
- Lower Firewall
### Silo Browsing & O365
- Lower Firewall
- Web Gateway (IAP Proxy)
## Technologies:

### NFVIS

NFVIS is designed specifically for enterprise deployments

- implement full lifecycle management from the **central orchestrator and controller** for the virtualized services, helps provision/launch, add/remove , stop/restart services(deployed as **VMs** or **VNFs**). monitor failures & recovery of services.

- segmentation of virtual networks, abstract computing resources
- API interface for REST and NETCONF
- Monitors NETCONF notification and the host and virtual machine statistics.
- Capture traffic transmitted and received over physical and virtual network interfaces controllers for analysis
- Design network topology using the GUI
- supports service chaining of 2 or more VMs 


### VNF
#### VNF Packing
#### NVF Networking
### ENCS HA
### Checkpoint HA
#### ClusterXL

- All machines in the cluster are aware of the connections passing through each of the other machines. The cluster members synchronize their connection and status information across a secure synchronization network.

- This Check Point cluster solution uses unique physical IP and MAC addresses for its cluster
members and virtual IP addresses to represent the cluster itself. 
- The virtual IP addresses do not belong to an actual machine interface, and it is recommended that each cluster member have at least three interfaces: one external interface, one internal interface, and one for synchronization

#### ClusterXL Modes

- Load Sharing Multicast Mode
- Load Sharing Unicast Mode
- High Availability Mode

### McAfee HA


### - ENCS 5400
Following are the software components on ENCS platform software that may require update.
- NFVIS (NFV Infrastructure software)
- CIMC (Cisco Intelligent Management controller) for platform management
- BIOS
- NIC Firmware

#### 1: Initial Setup
Connect with your computer using mgmt port then go to http://192.168.1.1 and use default crendentials then cahnge password and check current NFVIS version.
#### 2: Verification and device upgrade
Firmware: `show platform version` then check comptability if you can upgrade to newer versions.

> NFVIS upgrade procedure will update the other platform software components (CIMC, BIOS, NIC firmware), if required.

when upgrade to newer version, if bios is also upgraded, 2 reboots will be required.
to upgrade from 4.3.1 to 4.6.1, you have to upgrade version per version.
4.3.1 -> 4.4.1 -> 4.5.1 -> 4.6.1
then check status `show system upgrade apply-image`
#### 3: Network config
- configure mgmt interface ip for ENCS 1 & 2 
ENCS-1
```
config terminal
system settings mgmt ip address 192.168.100.1 255.255.255.0
system settings default-gw 192.168.100.254
commit
```
ENCS-2
```
config terminal
system settings mgmt ip address 192.168.100.3 255.255.255.0
system settings default-gw 192.168.100.254
commit
```
- configure CIMC interface ip for ENCS 1 & 2 

#### 4: Internal Network Configuration
- To protect against cable connection issue and box failure, there is back-to-back cable between ENCS and connection from each ENCS to the external switch
- => we'll use MSTP in ENCS and external switches and not RSTP because it will cause ports blocking in back to back connections 
#### 4.1: VLAN Config
create VLANS: 1,100,110,111,800,810,811,820,830,831,840,850,860,880,890,900,910,920

> Note: The VLAN configuration takes effect only if the global VLANs are also configured with the same values in interface vlan configuration = If you create a VLAN globally with a specific VLAN ID, name, and other attributes, and you want to use that VLAN on a particular interface, you need to configure the interface VLAN with the same VLAN ID, name, and other relevant settings.

ENCS does not support PVST because it blocks ports when using back-to-back connections, this will result in blocking traffic between VNFs and thus VNFs traffic will not work.



### - Checkpoint 6200
### - Checkpoint SMS
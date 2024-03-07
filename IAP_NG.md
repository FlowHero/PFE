

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

### Silo VPN & Remote Access

### Silo Partners
VPN Ipsec between the partner and the VPN Services on the ENCS. All flow coming from partners will transit by a specific firewall to restrict the access to internal server by the partners. Physical Netscaler will permut to have protocol break between the two lines of firewalls.
### Silo Browsing & O365

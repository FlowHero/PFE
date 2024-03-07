

## Goals de NG-IAP:

### Goals principales:
- Remplacage des hardwares **EoL** ou/et **Non-Compliant**
- **Standarisation** d'infrastucture IAP
- **Secure Browsing** securisation du browsing d'internet pour aligner avec les normes de conformité de *Darwin2 security* (
  Deciphering,
  Anti-Malware,
  Anti-Virus,
  DLP Inspections
)

>Darwin2 initiatives require deciphering browsing traffic to 
perform security inspections, such as antivirus, anti-malware and DLP.

### Autres:

- **Remote Access**
- **Cloud Access**


- Virtualization d'une partie d'IAP pour:
  Simplifier le logistique, MCO, energie et cout.

- Utilisation de Cisco ENCS pour:
  préparer pour le future dans levolution d'infrastructure

## GTS IAP Architecture:

- SG possède plusieurs IAP pour différents besoins, qui sont tous conformes avec standard de sécurité (ANSSI)pour éviter une *compromise* 
- y a des *IAP de hosting* qui permet au utilisateurs externes d'acceder au applications et ressources internes
- y a des *IAP Hybrid* qui permet d'avoir un flux entrant et sortant, utilisé par les SSL VPN et la partie Browsing

### Browsing

le browsing dans SG est établie d'une maniere purement professionel pour garantire la sécurité des SI et des utilisateurs

## Solution Overview

• Upper filtering and Admin layer will rely on physical hardware
• Cisco ENCS technology will be introduced to virtualize lower filtering and services layer.

| aze | azea|
| | |
|yes | no|
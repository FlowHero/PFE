
### **Encrypted traffic with no SSL Inspection**:

When a website has a trusted and verified certificate, malwares wont be blocked when clients tries to download them from that infected site.
That's where Deep SSL Inspection comes into place, which is used to inspect encrypted session

#### SSL Certificate Inspection:

 > Inspect certificate and Packet Header

Fortigate doesn't encrypt traffic during hello messages
Parses **SNI** (which holds the complete url of the SSL Server) and validate it against server DNS name before receipt of the server certificate.
    if there's no SNI, it looks for **Subject** or **SAN**

SSL Certificate Inspection security features:
- Web Filtering
- Application Control

Cons:
- Doesn't Allow fortigate to inspect flow of encrypted data.

#### SSL Full Inspection:

 > Inspect all packet content including the payload


![xaa](./images/Annotation%202024-03-14%20100555.png)


- it does it by proxying the SSL connection 
- 2 SSL Connections are established : Client-Fortigate, Fortigate-Server
- 2 established sessions allows Fortigate to encrypt/decrypt packets using its own keys
- => Thus, Full inspection of data inside the encrypted packets

![xa](./images/Annotation%202024-03-14%20101530.png)

=> Full inspection is good against attacks that use HTTPs and otehr SSL-Based protocols

![sshssl](./images/Annotation%202024-03-14%20102029.png)

![eadsq](./images/Annotation%202024-03-14%20102319.png)

![profaSSH](./images/Annotation%202024-03-14%20103342.png)

- Inbound traffic: Protecting SSL Server
- Output traffic: Multiple clients connecting to multiple servers

By default, Fortigate uses a self signed CA Certificate to re-encrypt traffic requited by SSL Full inspection. so How to avoid the "untrusted certificate" Warning ?
    - you can eiher use `Fortigate_CA_SSL` and install it on all of your company device browsers. 
    - or install on your fortigate a CA certificate that is signed by your company's CA, which will be already recognized by device browsers on your company.



### Exempting traffic

you may need to **exempt** traffic from ssl inpection if it is causing problems with traffic or for legal reasons.

For example it is illegal to inspect SSL Bank-related traffic.
Configuring exemption for sites is simpler than setting up firewall policies for each individual bank.

You can exempt based on *category* (**Finance and Banking**, **Health and Wellness**) or based on address.

you can use enable **Reputable websites** which holds a list of sites excluded that are maintained and updated by FortiGuard.

### Applying SSL Inspection profile to a Firewall Policy

![aeaz](./images/Annotation%202024-03-14%20114533.png)

**Decrypted traffic mirror** will send a copy of unencrypted SSL traffic to an interface.

### Certificate Warning on the FortiGate GUI

you can use one of the 2 solutions we used in Deep SSL Inspection or you can just click *Advanced* and trust the self-signed certificate.

Another option for companies who manage their own CA:
    generate a certificate for each of your FortiGate device and use them to secure your HTTPS connections.

You can use `Fortinet_GUI_Server` Certificate which is signed by the `Fortinet_CA_SSL` to avoid browser warnings on HTTPS access to fortigate GUI, you must import `Fortinet_CA_SSL` certificate into your management devices.

Download `Fortinet_CA_SSL` from fortigate first, then import it on your devices as a CA Authority

> 📌Certificate for FortiGate GUI 

> 📌 Certificate for SSL Inspection

you can exempt microsoft exchange server addresses from ssl inspection. While delete the error, you are no longer performing SSL Inspection on email traffic

#### Invalid certificates

you can select a custom action on each type of invalid certificates as shown below:

![crtinv](./images/Annotation%202024-03-22%20093757.png)

Fortigate will then present the certificate as either **trusted** (signed by `Fortinet_CA_SSL` ) or **untrusted** (signed by `Fortinet_CA_Untrusted` ),

- Untrusted certificates includes self-signed certificates unless the certificate is imported into the browser certificate store.

![bai](./images/Annotation%202024-03-22%20094908.png)

- Allow:
    - if *untrusted*, Fortigate signs the certificate using `Fortinet_CA_Untrusted` and sends the temporary certificate to the browser and a warning will appear to the user indicating that the site is untrusted.
    -  Otherwise if the certificate is *trusted*, it is signed by `Fortinet_CA_SSL` and send the temporary certificate to the browser

- Ignore:
    - Trusted or Untrusted; it is signed by  `Fortinet_CA_SSL` and temporary cert is sent to the browser

### HSTS

is a security feature designed to detect MITM Attacks by mking sure that any certificate presented when accessing a server resource is signed by a specific CA

if the browser detects any other CA, it simply refuses to continue the SSL Handshake, and prevent access to the website 

![hsts](./images/Annotation%202024-03-22%20100433.png)

When replacing certificate for traffic causes problems and prevent users fro maccessing some websites, solutions are limited:

![deepexempt](./images/Annotation%202024-03-22%20101117.png)


#### Solution 1

- Fortigate level: Exempt affected traffic from full SSL inspection and use certificate inspection instead


#### Solution 2

- Browser level: disable HSTS validation per website or globally  


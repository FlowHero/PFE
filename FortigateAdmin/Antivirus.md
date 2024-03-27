## Antivirus

With a valid fortigate liscnese, fortigate can update Viruses signated from Fortiguard servers


![av1](./images/Annotation%202024-03-22%20101526.png)

Fortigate can operate in **flow-based** or **proxy-based** inspection mode.

### Flow-Based

uses hybrid of 2 scanning modes:

- **Default scanning mode**:
enhances the scanning of nested archive files without buffering the container archive file

- **Legacy scanning mode**:
 buffers the full container then scans it


![av2](./images/Annotation%202024-03-22%20102321.png)

client send a request and starts receiving packets immediatly.
Fortigate will also caches those packets,
when last packet received, Fortigate caches it and put it on hold, IPS engine will extract the payload and send it to AV scanning engine,
if it's clean, the packet is transmitted to the client
if it's a virus, the packet is dropped, connection is reset, and the client will not receive the last packet. Even though he has received most of the packets, he can't open the file because he has been truncated 
IPS engine will cache the url of infected file so that next time it isn't scanned but blocked.

Because file is transmitted at the same time, flow-based consume more CPUs, however using some FortiGate models you can offload some operations to SPUs.

#### How to configure Flow-Based inspection mode ?

> It's the default mode !

1 - Create AV Profile with the selection of inspected protocols, and the action to take when a virus infected file is detected.

2 - Apply AV Profile to a Firewall policy

### Proxy-Based Inspection mode

![av4](./images/Annotation%202024-03-22%20104122.png)

im too tired to write, i'll just watch.

#### Protocol options

This is used with proxy-mode inspection, primarily to declare which inspectable protocols are to be checked for on which ports. Not used in flow-mode (IPS engine detects the app), nor if "inspect all ports" is enabled (also identified by IPS engine).

### Log and monitor antivirus events
### Troubleshoot common antivirus issues

## Web Filtering

blocking a domain is considered too much than blocking url;
for example you can block per author:
tumblr.com/hacking instead of whole site

In the default profile based mode, Fortigate provides 2 inspections modes ( Flow-Based & Proxy-Based) to perform web filtering.

![av6](./images/Annotation%202024-03-22%20113326.png)

Proxy-based inspection is **transparent** because in ip layer, fortigate is not the destination address, but it just intercept the traffic.

![av7](./images/Annotation%202024-03-22%20114011.png)

If encrypted protocols, fortigate requires additional inspection.
when using ssl certificate inspection, Fortigate doesn't decrypt or inspect any encrypted traffic, it just exxtract SNI or CN from the SSL handshakes (because they are not encrypted) and try to rate the site using the domain extracted.

but sometimes SNI is not same as CN field (example of youtube and google)

- **Server SNI Check**
    - `Enable`: Fortigate **Uses** the domain in CN field instead of that in SNI field if domain(CN)#domain(SNI) and domain(SAN)
    - `Strict`: Fortigate **Closes** Connection if there's a mismatch
    - `Disable`: Fortigate always rate url based on FQDN


![xaeaz](./images/Annotation%202024-03-27%20093838.png)

### FortiGuard web Filter

![xaez](./images/Annotation%202024-03-27%20094907.png)

FortiGuard looks at category that website has been rated with and take action based on that category.

- **Monitor**: Allow access to site and logs it at same time
- **Warning**: You choose to access or go back
- **Authenticate**:
    - Blocks requested website unless you entered the right login credentials

    - choosing this action allow you to define user groups tat are allowed to override the block 

if you feel a url doesn't have the right category, you can override it


Static URL Filtering
![azesx](./images/Annotation%202024-03-27%20100042.png)

Exempt: allow traffic from trusted sources to bypass all security inspections.

![azesq](./images/Annotation%202024-03-27%20101018.png)

for each step, if there's no match, fortigate moves to the next step enabled.
Advanced Filters = Safe Search or remove active X components

Category based filtering requires a live connection to FortiGuard  

you can enable **Web Filter Cache** on fortiGate for fortiguard so that the rating of a url that is already known, doesn't have to send a rating request but just perform a memory lookup, which is fatser than packets travelling.

![XGZER](./images/Annotation%202024-03-27%20102147.png)

## IPS
To get maximum IPS features, combine IPS with deep ssl inspection

## Application Control

when you want to block torrents, for example. Because they use dynamic ports and `P2P` traffic, it's extremely difficult to block with a regular firewall rule. This is where **application control** comes in. It uses the IPS engine to inspect the traffic and block it.

you can instead of blocking all facebook in web filtering, block just Facebook App while allowing Facebook chat for collaboration.

**Application control** provides more flexibility in controlling access to web-based or other applications than "simple" **web filtering** can.


> Application control occurs after the web filtering so that web filtering exempt cannot bypass app control.
### Flow-Based web filtering


### Proxy-Based web filtering 


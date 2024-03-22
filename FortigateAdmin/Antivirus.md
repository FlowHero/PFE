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

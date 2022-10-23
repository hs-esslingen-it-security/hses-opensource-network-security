# PacketFence
This document highlights possibilities to automate PacketFence from a SIEM system. PacketFence offers a [REST API](https://www.packetfence.org/doc/api/)

## Authentication
The Packet API requires authentication with a token. With the following bash script, the token can be extracted:
```bash
PACKETFENCE_IP="127.0.0.1"
PACKETFENCE_USER="admin"
PACKETFENCE_PASSWORD="password"
curl -X POST "https://$PACEKTFENCE_IP:9999/api/v1/login" -H  "accept: application/json" -H  "Content-Type: application/json" -d "{\"username\":\"$PACKETFENCE_USER\",\"password\":\"$PACKETFENCE_PASSWORD\"}" | python3 -m json.tool
```

## Isolation of a Device
NAC systems, i.e., PacketFence, have control on the assignment of devices to specific VLANs based on certain criteria, e.g. 802.1X authentication. However, PacketFence has only limited capability to analyze the devices. For example PacketFence can automate OpenVAS scans to check for externally visible vulnerabilities, but cannot analyze the filesystem.
If an HIDS detects issues in the filesystem, a SIEM has the capability to react, but cannot move the device to a different VLAN for isolation. Hence, the SIEM needs to trigger the NAC to do so.

In the following example script, the MAC Address of the end-device is known and the device is unregistered from the previosly assigned role.

```bash
PACKETFENCE_IP="127.0.0.1"
PACKETFENCE_TOKEN="AUTH_TOKEN"
MAC="AA:BB:CC:DD:EE:FF"
curl -X PATCH "https://$PACKETFENCE_IP:9999/api/v1/node/$MAC" -H  "accept: application/json" -H "Authorization: $PACKETFENCE_TOKEN" -H "Content-Type: application/json" -d "{\"status\":\"unreg\"}”
```

## Triggering of Security Events
Alternatively, PacketFence can execute predifined routines for devices. Such a routine could contain unregistering a device or starting an OpenVAS scan. These routines are called security event and identified via a numerical ID. PacketFence has predefined security events, and allows adding customized events.

```bash
PACKETFENCE_IP="127.0.0.1"
PACKETFENCE_TOKEN="AUTH_TOKEN"
MAC="AA:BB:CC:DD:EE:FF"
SECURITY_EVENT=1234
curl -X POST "https://$PACKETFENCE_IP:9999/api/v1/node/$MAC/apply_security_event" -H  "accept: application/json" -H "Authorization: $PACKETFENCE_TOKEN" -H "Content-Type: application/json" -d "{\"security_event_id\":\"$SECURITY_EVENT\"}”
```

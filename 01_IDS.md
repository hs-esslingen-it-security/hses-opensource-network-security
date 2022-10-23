# Intrustion Detection System (IDS)
This document summarizes the IDSs evaluated in this work. We distinguish between Host-IDS and Network-IDS.

## Host-IDS (HIDS)
HIDS typically run as an agent on an end-device. They have the capability to collect data and monitor processes and the filesystem. The collected results are reported to a SIEM system or stored in log files.

### Wazuh Agent
* The Wazuh Agent requires a Wazuh Server ([see SIEM](02_SIEM.md))
* The Wazuh Agent and Server need to be able to communicate on an IP base
* [Installation of Wazuh Agent](https://documentation.wazuh.com/current/installation-guide/#installing-the-wazuh-agent)

### OSSEC Agent
* Offers e.g., rootkit detection and file integrity checks
* Integrates with OSSEC Server and AlienVault OSSIM
* [Installation of OSSEC Agent](https://www.ossec.net/download-ossec/)


## Network-IDS (NIDS)
NIDS observe network traffic. This can be done on an end-device or on a network infrastructure device. For network infrastructure devices, this either is included in the forwarding path, or all traffic is mirrored to a dedicated NIDS device.

### Snort
* Snort filters traffic based on rules
* [Installation of Snort](https://www.snort.org/documents)

### Suricata
* Suricata operates on Snort rules and has additional syntax features for e.g., Application Layer Detection Rules
* [Installation of Suricata](https://suricata.readthedocs.io/en/latest/install.html)

### Zeek
* Zeek is based on signatures and anomalies in traffic
* [Installation of Zeek](https://docs.zeek.org/en/lts/install.html)

### OwlH
* OwlH itself is no NIDS! It is just a management system for multiple NIDS systems, e.g., Hosts with Suricata or Zeek.
* OwlH has the capability to manage rule sets and across multiple NIDS systems. These rulesets can be edited manually or automatically downloaded from specified sources.
* [Installation of OwlH](https://documentation.owlh.net/en/0.17.0/main/install/ins-owlh-aio.html)
* OwlH can directly integrate all connected NIDS into the Wazuh Server ([see SIEM](02_SIEM.md))
* [Integration into Wazuh](https://documentation.owlh.net/en/0.17.0/main/OwlHWazuh.html)

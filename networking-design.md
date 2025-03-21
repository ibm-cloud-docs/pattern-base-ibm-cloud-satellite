---

copyright:
  years: 2024
lastupdated: "2024-02-20"

subcollection: pattern-base-ibm-cloud-satellite

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

Networking is a key aspect of any cloud deployment that should not be overlooked. The network design should consider enterprise connectivity, latency, throughput, resiliency, and security requirements. The following items are some of the key requirements from a network perspective:

- Hosts must have minimum network bandwidth connectivity of 100 Mbps, with 1 Gbps preferred.
- Host IP addresses must remain static and cannot change over time.
- All {{site.data.keyword.satelliteshort}} hosts must have an IPv4 address since {{site.data.keyword.satelliteshort}} does not support IPv6. For more information, see [Host network requirements](/docs/satellite?topic=satellite-reqs-host-network).


The table shows IP address ranges that are reserved for {{site.data.keyword.satellitelong_notm}} and should not be used for any other purpose.
| Location type | IP range |
|---|---|
| Non-CoreOS enabled locations | 172.16.0.0/16, 172.18.0.0/16, 172.19.0.0/16, 172.20.0.0/16, and 192.168.255.0/24 |
| CoreOS enabled locations | 172.20.0.0/16 and 172.16.0.0/16 |
{: caption="Reserved IP Addresses" caption-side="bottom"}


{{site.data.keyword.satellitelong_notm}} link securely connects a Satellite location to the IBM Cloud region that manages the location. Communication to and from the Satellite location is proxied by the Link tunnel server. Satellite link uses a zero trust model and all communication over [Satellite link](/docs/satellite?topic=satellite-link-location-cloud) is encrypted by IBM.

There is also the Satellite Connector that enables only the secure communications from IBM Cloud to on-prem resources with a light-weight container that is deployed on the container platform hosts. See [Satellite Connector overview](/docs/satellite?topic=satellite-understand-connectors).
Satellite Connector replaces [Satellite Gateway](/docs/satellite?topic=satellite-connector-and-secure-gateway).

One could use a secure IBM Cloud® Direct Link connection for Satellite Link communications between customer services running in an {{site.data.keyword.satellitelong_notm}}® Location and IBM Cloud®. Note, this option is ony available with Red Hat CoreOS-enabled Satellite locations.

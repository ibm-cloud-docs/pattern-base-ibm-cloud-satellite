---

copyright:
  years: 2024
lastupdated: "2024-2-20"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for the deployment of {{site.data.keyword.satelliteshort}} on-premises or in hyperscaler pattern.

## Architecture decisions for high availability
{: #high-availability}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| High availability deployment| {{site.data.keyword.satelliteshort}} host nodes: Control and worker nodes | - Single zone deployment \n Multi-zone deployment	| Multi-zone deployment	| Minimum of 3 hosts and spares across the 3 zones Place the host machines in physically different racks. Power, network, and storage isolation and separate data centers recommended for protection against outages for any of these components. Separate physical locations (<100 ms latency) are recommended for protection against data center outages. For more information, see [{{site.data.keyword.satelliteshort}} HA considerations](/docs/satellite?topic=satellite-ha). |
| High Availability | Red Hat OpenShift workloads | \n - Single zone Red Hat OpenShift cluster \n - Multi-zone Red Hat OpenShift cluster | Multi-zone Red Hat OpenShift cluster | Configure Red Hat OpenShift clusters with a minimum of 3 worker nodes and spares across 3 zones. Size the worker nodes in each zone at 50% of required CPU capacity for workloads to meet 100% capacity requirements because of a zone failure. |
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-and-restore}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| Backup |  {{site.data.keyword.satelliteshort}} control plane data | {{site.data.keyword.IBM_notm}} managed backups in {{site.data.keyword.cos_full_notm}} |{{site.data.keyword.IBM_notm}} managed backups in {{site.data.keyword.cos_full_notm}}| {{site.data.keyword.IBM_notm}} {{site.data.keyword.satelliteshort}} service backs up {{site.data.keyword.satelliteshort}} control plane data as follows. For more information, see [Securing your Data](/docs/satellite?topic=satellite-data-security) \n {{site.data.keyword.satelliteshort}} control plane master data backups in {{site.data.keyword.IBM_notm}} owned {{site.data.keyword.cos_full_notm}} instance every hour \n {{site.data.keyword.satelliteshort}} enabled services master data backups in customer-owned {{site.data.keyword.cos_full_notm}} instance every 8 hours |
| Backup | OpenShift clusters | - Portworx PX backup for Kubernetes \n Kasten by Veeam \n Triliovault for Kubernetes \n Bring your own backup tool | Portworx PX Backup for Kubernetes | Use the PX-Backup add-on to Portworx Enterprise to backup application data, configuration, and Kubernetes objects at the Kubernetes pod, namespace, or cluster level. \n Backups can be stored in a customer-owned {{site.data.keyword.cos_full_notm}} instance.|
{: caption="Table 2. Backup and restore architecture decisions" caption-side="bottom"}

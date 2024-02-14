---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for base {{site.data.keyword.satellitelong_notm}} pattern.

## Architecture decisions for high availability
{: #high-availability}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | High availability deployment \n  {{site.data.keyword.satelliteshort}} host nodes \n  Control and worker nodes | - Single zone deployment \n- Multi-zone deployment	| Multi-zone deployment	| Minimum of 3 hosts and spares across the 3 zones (physically separate racks). Power, network, and storage isolation and separate data centers recommended for protection against outages for any of these components. Separate physical locations (<100ms latency) recommended for protection against data center outages. For more information, see [{{site.data.keyword.satelliteshort}} HA considerations](/docs/satellite?topic=satellite-ha). |
| 2 | High Availability \n  OpenShift workloads | \n  Single zone OpenShift cluster \n  Multi-zone OpenShift  luster | Multi-zone OpenShift  luster | Configure OpenShift clusters with a minimum of 3 worker nodes and spares across 3 zones. Size the worker nodes in each zone at 50% of required CPU capacity for workloads to meet 100% capacity requirements in the event of a zone failure. |
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-and-restore}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | Backup \n  {{site.data.keyword.satelliteshort}} control plane data | {{site.data.keyword.IBM_notm}} managed backups in {{site.data.keyword.cos_full_notm}} |{{site.data.keyword.IBM_notm}} managed backups in {{site.data.keyword.cos_full_notm}}| {{site.data.keyword.IBM_notm}} {{site.data.keyword.satelliteshort}} service backs up {{site.data.keyword.satelliteshort}} control plane data as follows. For more information, see [Securing your Data](/docs/satellite?topic=satellite-data-security)): \n {{site.data.keyword.satelliteshort}} control plane master data backups in {{site.data.keyword.IBM_notm}} owned {{site.data.keyword.cos_full_notm}} instance every hour \n {{site.data.keyword.satelliteshort}} enabled services master data backups in customer-owned {{site.data.keyword.cos_full_notm}} instance every 8 hours |
| 2 | Backup \n  OpenShift clusters | - Portworx PX Backup for Kubernetes \n Kasten by Veeam \n Triliovault for Kubernetes \n- Bring your own backup tool | Portworx PX Backup for Kubernetes | Use PX-Backup add-on to Portworx Enterprise to backup application data, configuration, and Kubernetes objects at the Kubernetes pod, namespace, or cluster level. \n  Backups can be stored in a customer-owned {{site.data.keyword.cos_full_notm}} instance.|
{: caption="Table 2. Backup and restore architecture decisions" caption-side="bottom"}

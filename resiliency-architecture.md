---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for base IBM Cloud Satellite pattern.

## Architecture decisions for high availability
{: #high-availability}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | High Availability Deployment \n  Satellite Host Nodes \n  (control and worker nodes) | - Single zone deployment \n- Multi-zone deployment	| Multi-zone deployment	| Minimum of 3 hosts+spares across 3 “zones” (physically separate racks). Power, network, and storage isolation and separate data centers recommended for protection against outages of any of these components. Separate physical locations (<100ms latency) recommended for protection against data center outages. See [Satellite HA considerations](https://cloud.ibm.com/docs/satellite?topic=satellite-ha). |
| 2 | High Availability \n  OpenShift Workloads | \n  Single zone OpenShift Cluster \n  Multi-zone OpenShift Cluster | Multi-zone OpenShift Cluster | Configure OpenShift clusters with a minimum of 3 worker nodes+spares across 3 zones. Size worker nodes in each zone at 50% of required CPU capacity for workloads to meet 100% capacity requirements in the event of a zone failure. |
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-and-restore}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | Backup \n  Satellite Control Plane Data | IBM-managed backups in Cloud Object Storage | IBM-managed backups in Cloud Object Storage | IBM Satellite Service backs up Satellite Control Plane data as follows (see [Securing your Data](https://cloud.ibm.com/docs/satellite?topic=satellite-data-security)): \n- Satellite Control plane master data backups in IBM-owned COS instance every hour \n- Satellite-enabled services master data backups in customer-owned COS instance every 8 hours |
| 2 | Backup \n  OpenShift Clusters | - Portworx PX Backup for Kubernetes \n- Kasten  by Veeam \n- Triliovault for Kubernetes \n- BYO backup tool | Portworx PX Backup for Kubernetes | Use PX-Backup add-on to Portworx Enterprise to backup application data, configuration, and Kubernetes objects at the Kubernetes Pod, Namespace, or Cluster level. \n  Backups can be stored in a customer-owned IBM Cloud Object Storage instance.|
{: caption="Table 2. Backup and restore architecture decisions" caption-side="bottom"}

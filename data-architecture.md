---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for data
{: #data-architecture}

<!-- Below is a placeholder for all compute domain decisions.  Remove the domains that are not in scope.  If there are decisions
that need to be added (e.g. platform dependent) add additional rows-->

The following tables summarize the data architecture decisions for the {{site.data.keyword.satellitelong_notm}} base pattern.

## Architecture decisions for data
{: #data-residency}

The following are architecture decisions for data for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| Data Residency | Data should stay in the region | - Store data in Satellite location \n- Store data in IBM Cloud | Store data in Satellite Location | Deploy Cloud Object Storage (COS) in a Satellite location with sufficient computing hosts and raw block storage allocated for provisioning Object Storage. \n  Enable IBM Cloud Databases (ICD) for Satellite and deploy PostgreSQL or Redis ICD instances into a Satellite location. \n  See [on-premises COS](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-about-cos-satellite) and [on-premises SAN storage](https://cloud.ibm.com/docs/cloud-databases?topic=cloud-databases-satellite-on-prem). |
{: caption="Table 1. Architecture decisions for data" caption-side="bottom"}

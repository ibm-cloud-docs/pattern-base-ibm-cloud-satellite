---

copyright:
  years: 2024
lastupdated: "2024-02-13"

subcollection: pattern-base-ibm-cloud-satellite

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}

Resiliency is best achieved by deploying more than one {{site.data.keyword.satelliteshort}} locations and duplicating the configuration.

High availability in a {{site.data.keyword.satellitelong_notm}} solution architecture can be achieved on 3 levels - {{site.data.keyword.satellitelong_notm}} Management plane, {{site.data.keyword.satellitelong_notm}} Control plane, IBM Cloud services. See [{{site.data.keyword.satelliteshort}} Link](docs/satellite?topic=satellite-ha).


| Component Level | Description | Comments |
|---|---|---|
| {{site.data.keyword.satelliteshort}} Management plane nodes | Choose an IBM Cloud multizone metro that runs and manages the {{site.data.keyword.satelliteshort}} control plane of your location. IBM provides high availability | By default, every {{site.data.keyword.satelliteshort}} control plane is automatically set up with multiple instances and spread across multiple zones within the same IBM Cloud multizone metro.|
| {{site.data.keyword.satelliteshort}} Control plane nodes | Ensure the compute hosts in the {{site.data.keyword.satelliteshort}} location are set up highly available. HA setup ensures that the {{site.data.keyword.satelliteshort}} control plane continues to run, even if compute hosts experience a power, networking, or storage outage | Deploy compute hosts multiples of 3, such as 6, 9, or 12. n\  Every compute host must have a separate physical host n\  In a hyperscaler spread compute hosts across many availability zones within the same metro.|
| IBM Cloud Services | Every IBM Cloud service running in the Satellite location, like Red Hat OpenShift on IBM Cloud, comes with a set of options for how to increase service availability | Review documentation of each service to find supported options |
{: caption="Table 1. High availability at different levels" caption-side="bottom"}

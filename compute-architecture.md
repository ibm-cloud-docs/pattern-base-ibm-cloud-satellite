---

copyright:
  years: 2024
lastupdated: "2024-02-13"

subcollection: pattern-base-ibm-cloud-satellite

keywords: IBM Cloud Satellite, location, Satellite, base

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

The following sections summarize the compute architecture decisions for the deployment of {{site.data.keyword.satelliteshort}} on-premises or in hyperscaler pattern.

| Architecture decision| Requirement| Options |Decision| Rationale|
|---|---|---|---|---|
| Compute: Hosts |{{site.data.keyword.satelliteshort}} hosts | {{site.data.keyword.baremetal_short}} \n Virtual machine (VM) | VM | {{site.data.keyword.satelliteshort}} hosts represent the compute machine on the selected infrastructure. In this solution, the {{site.data.keyword.satelliteshort}} hosts are virtual machines from an existing Kernel-based Virtual Machine or OpenStack environment that deploys at the customer’s on-premises locations. |
| |Host OS| RHEL 8.x (min) \n RH Core OS | RHEL 8.x | {{site.data.keyword.satelliteshort}} requires RHEL 7.x minimal image (no LAMP stack) on x86. Bring Your Own RHEL license. For the latest specs, see [{{site.data.keyword.satelliteshort}} Host System Requirements](/docs/satellite?topic=satellite-host-reqs) |
| |Control plane hosts| 4 vCPU and 16 GB RAM \n 16 vCPU and 64 GB RAM | 4 vCPU and 16 GB RAM | Minimum of 3 host nodes (Minimum of 6 hosts for RHCOS); multiples of 3, for example, 6, 9, 12. The configuration and number of control planes hosts that are needed depends on the number of clusters (for customer workloads and {{site.data.keyword.satelliteshort}}-enabled services) deployed at the {{site.data.keyword.satelliteshort}} location and the total number of worker nodes across all clusters. For more information, see [{{site.data.keyword.satelliteshort}} Location Sizing](/docs/satellite?topic=satellite-about-locations)|
| |{{site.data.keyword.satelliteshort}} services worker node hosts: Red Hat OpenShift| 4 vCPU and 16 GB RAM \n 16 vCPU and 64 GB RAM | 16 vCPU and 64 GB RAM | A minimum of 3 host nodes and it's recommended to have spare nodes in multiples of 3 (for example 6, 9, 12). Larger configuration used in this solution to support converged compute and storage requirements. The number of worker node hosts depends on the customer workloads to be deployed in the clusters. For more information, see [Sizing your Red Hat OpenShift cluster to support your workload](/docs/openshift?topic=openshift-strategy). |
| |{{site.data.keyword.satelliteshort}} services worker nodes hosts: Other services| 4 vCPU and 16 GB RAM \n 16 vCPU and 64 GB RAM | For more information, see [{{site.data.keyword.satelliteshort}}-Enabled Services](/docs/satellite?topic=satellite-managed-services) | Review the requirements for each [Satellite-Enabled Service](/docs/satellite?topic=satellite-managed-services). |
| Compute: Containers | Compute: Containers| Managed Red Hat OpenShift on {{site.data.keyword.satelliteshort}} \n Bring Your Own Red Hat OpenShift on-premises | Managed Red Hat OpenShift on {{site.data.keyword.satelliteshort}} | Ease of provisioning and managed control plane \n Consistent deployment of Red Hat OpenShift clusters across customer locations |
| |Red Hat OpenShift Cluster: Container images registry | {{site.data.keyword.registrylong}} \n OpenShift Container Registry \n BYO image registry | {{site.data.keyword.registryfull_notm}} | By default, the Red Hat OpenShift internal registry is disabled because it does not have backing storage. {{site.data.keyword.registryfull_notm}} is a highly available private registry that can be accessed from multiple clusters and provides vulnerability scanning of the images. |
| |Red Hat OpenShift cluster: Connectivity | Private service cluster URL \n Public service cluster URL | Private service cluster URL | By default, the Red Hat OpenShift cluster that runs in the location can be accessed through the service cluster URL from only the {{site.data.keyword.satelliteshort}} location’s private network. \n  if the {{site.data.keyword.satelliteshort}} location hosts have public internet access, the service cluster URL can be updated to enable access to the cluster from the public network. This is not recommended for production workloads. For more information, see [Host Network Connectivity](/docs/openshift?topic=openshift-sat-expose-apps). |
| |Red Hat OpenShift cluster: Workloads access | Red Hat OpenShift routes \n Node ports | Red Hat OpenShift routes | Expose apps to requests from the public or a private network with a hostname (from Red Hat OpenShift Ingress controller's external IP address). Support HTTP and HTTPS protocols only. If the worker node hosts have public network connectivity, the cluster is created with a public Ingress controller by default. If the worker node hosts have private network connectivity only, the cluster is created with a private Ingress controller by default. |
| | | | Node ports | Expose non-HTTP(S) apps, for example, User Datagram Protocol or Transmission Control Protocol (TCP) apps, with a NodePort in the 30000-32767 range. |
| |Red Hat OpenShift cluster: Workload isolation | Single cluster for all workloads \n Separate clusters per workload | Single cluster for all workloads | Single cluster for all workloads. Workload isolation is achieved through projects and namespaces within a cluster. For more information, see [Container network policies](/docs/openshift?topic=openshift-network_policies), and IAM access roles. |
{: caption="Table 1. Architecture decisions for compute" caption-side="bottom"}

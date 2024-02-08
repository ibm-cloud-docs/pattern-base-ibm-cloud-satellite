---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

The following sections summarize the compute architecture decisions for base IBM Cloud Satellite pattern.

<!-- below is a placeholder for all compute domain decisions  Remove the domains that are not in scope.  If there are decisions that need to be added (e.g. platform dependent) add additional rows-->

| Architecture decision| Requirement| Options |Decision| Rationale|
|---|---|---|---|---|
| Compute: Hosts |Satellite Hosts | Bare Metal Server \n  Virtual Machine (VM) | VM | Satellite hosts represent the compute machine on the selected infrastructure. In this solution, the Satellites hosts are Virtual Machines from existing KVM/OpenStack environment deployed at the customer’s on-prem locations. |
| |Host OS| RHEL 8.x (min) \n  RH Core OS | RHEL 8.x | Satellite requires RHEL 7.x minimal image (no LAMP stack) on x86. BYO RHEL license.  For the latest specs see [Satellite Host System Requirements](https://cloud.ibm.com/docs/satellite?topic=satellite-host-reqs) |
| |Control Plane Hosts| 4 vCPU and 16GB RAM \n  16 vCPU and 64GB RAM | 4 vCPU and 16GB RAM | Minimum 3 host nodes (Min 6 hosts for RHCOS); multiples of 3 (e.g. 6, 9, 12). The configuration and number of control planes hosts needed depend on the number of clusters (for customer workloads and Satellite-enabled services) deployed at the Satellite location and the total number of worker nodes across all clusters. See [Satellite Location Sizing](https://cloud.ibm.com/docs/satellite?topic=satellite-about-locations) for more information. |
| |Satellite Services Worker Nodes Hosts: OpenShift| 4 vCPU and 16GB RAM \n  16 vCPU and 64GB RAM | 16 vCPU and 64GB RAM | Minimum 3 host nodes+spare nodes recommended; multiples of 3 (e.g. 6, 9, 12). Larger configuration used in this solution to support converged compute/storage requirements. The number of worker node hosts depends on the customer workloads to be deployed in the clusters. See [Sizing your Red Hat OpenShift cluster to support your workload](https://cloud.ibm.com/docs/openshift?topic=openshift-strategy). |
| |Satellite Services Worker Nodes Hosts: Other Services| 4 vCPU and 16GB RAM \n  16 vCPU and 64GB RAM | See [Satellite-Enabled Services](https://cloud.ibm.com/docs/satellite?topic=satellite-managed-services) | See requirements for each [Satellite-Enabled Service](https://cloud.ibm.com/docs/satellite?topic=satellite-managed-services). |
| Compute: Containers | Compute: Containers| Managed OpenShift on Satellite \n  BYO OpenShift on-prem | Managed OpenShift on Satellite | Ease of provisioning and managed control plane \n  Consistent deployment of OpenShift clusters across customer locations |
| |OpenShift Cluster: Container Images Registry | IBM Container Registry on IBM Cloud \n  OpenShift Container Registry \n  BYO Image Registry | IBM Container Registry on IBM Cloud | By default, the OpenShift Internal Registry is disabled because it does not have backing storage. IBM Container Registry on IBM Cloud is a highly available private registry that can be accessed from multiple clusters and provides vulnerability scanning of the images. |
| |OpenShift Cluster: Connectivity | Private Service Cluster URL \n  Public Service Cluster URL | Private Service Cluster URL | By default, the OpenShift Cluster running in the location can be accessed through the Service Cluster URL only from the Satellite location’s private network. \n  If the Satellite Location hosts have public internet access, the Service Cluster URL can be updated to enable access to the cluster from the public network (not recommended for production workloads). See [Host Network Connectivity](https://cloud.ibm.com/docs/openshift?topic=openshift-sat-expose-apps) |
| |OpenShift Cluster: Workloads Access | OpenShift Routes \n  Node Ports | OpenShift Routes | Expose apps to requests from the public or a private network with a hostname (from OpenShift Ingress controller's external IP address). Support HTTP and HTTPS protocols only. If the worker node hosts have public network connectivity, the cluster is created with a public Ingress controller by default. If the worker node hosts have private network connectivity only, the cluster is created with a private Ingress controller by default. |
| | | | Node Ports | Expose non-HTTP(S) apps, e.g, UDP or TCP apps, with a NodePort in 30000-32767 range. |
| |OpenShift Cluster: Workload Isolation | Single cluster for all workloads \n  Separate clusters per workload | Single cluster for all workloads | Single cluster for all workloads. Workload isolation achieved through projects/name spaces within a cluster, [Container Network Policies](https://cloud.ibm.com/docs/openshift?topic=openshift-network_policies), and IAM access roles. |
{: caption="Table 1. Architecture decisions for compute" caption-side="bottom"}

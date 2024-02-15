---

copyright:
    years: 2024
lastupdated: "2024-02-08"

keywords: # Not typically populated

subcollection: pattern-base-ibm-cloud-satellite


authors:
    - name: Ashok Iyengar
      url: https://linkedin.com/in/ashokiyengar

version: 1.0

# Use if the reference architecture has deployable code.
# Value is the URL to land the user in the IBM Cloud catalog details page for the deployable architecture.
# See https://test.cloud.ibm.com/docs/get-coding?topic=get-coding-deploy-button
deployment-url:

docs: https://cloud.ibm.com/docs/pattern-base-ibm-cloud-satellite

# use-case from 'code' column in
# https://github.ibm.com/digital/taxonomy/blob/main/topics/topics_flat_list.csv
use-case: Managed cloud

content-type: reference-architecture

---

{{site.data.keyword.attribute-definition-list}}


# Base {{site.data.keyword.satellitelong_notm}} 
{: #base-ibm-cloud-satellite}
{: toc-content-type="reference-architecture"}
{: toc-use-case="Managed cloud"}
{: toc-version="1.0"}

The Base {{site.data.keyword.satellitelong_notm}} includes two common solution patterns:
- One or more {{site.data.keyword.satellitelong_notm}} locations configured on-premises
- One {{site.data.keyword.satelliteshort}} location is configured on-premises, and another {{site.data.keyword.satelliteshort}} location is configured in the cloud

Due to privacy, regulatory, or compliance reasons, customers might not send or store data in the public cloud. In such scenarios, the best option is to create one or more {{site.data.keyword.satelliteshort}} locations on-premises and hosts the data locally to satisfy the data residency requirement.

## Architecture diagrams
{: #architecture-diagram}

Figure 1 illustrates the {{site.data.keyword.satellitelong_notm}} architecture where one or more {{site.data.keyword.satelliteshort}} locations are deployed on-premises.


![Satellite location on-premises architecture](/images/SatLoc-on-premises-architecture.svg){: caption="Figure 1. Base {{site.data.keyword.satellitelong_notm}} solution architecture with {{site.data.keyword.satelliteshort}} location on-premises" caption-side="bottom"}

{{site.data.keyword.satelliteshort}} link connects on-premises {{site.data.keyword.satelliteshort}} locations to {{site.data.keyword.Bluemix_notm}}. Customers might also choose to use Direct Link. Red Hat OpenShift and Red Hat OpenShift Data Foundation are two of the many other {{site.data.keyword.satelliteshort}}-enabled services in this image that are deployed in the {{site.data.keyword.satelliteshort}} location.

Figure 2 illustrates the hybrid {{site.data.keyword.satellitelong_notm}} architecture where one or more {{site.data.keyword.satelliteshort}} locations are deployed on-premises and the other {{site.data.keyword.satelliteshort}} location is deployed in another cloud. The figure shows AWS as the hyperscaler.

![Satellite location hybrid architecture](/images/SatLoc-hybrid-architecture.svg){: caption="Figure 2. Base {{site.data.keyword.satellitelong_notm}} Solution Architecture with Hybrid Satellite locations" caption-side="bottom"}

A {{site.data.keyword.satelliteshort}} location can be created in {{site.data.keyword.Bluemix_notm}}, AWS, Azure, or Google. In this example one location is shown on AWS, while the other {{site.data.keyword.satelliteshort}} location is on-premises.

## Design scope
{: #design-scope}

The base {{site.data.keyword.satellitelong_notm}} solution covers design considerations and architecture decisions for the following aspects and domains:

- **Compute:** Bare Metal, Virtual Servers, Virtualization, Containers

- **Storage:** Primary Storage, Backup Storage

- **Networking:** Enterprise Connectivity, Network Segmentation

- **Data:** Data Storage (highlighting the Data Residency requirement)

- **Security:** Data Security, Identity and Access Management, Infrastructure and Endpoint

- **Resiliency:** Backup and Restore, High Availability

- **Service Management:** Monitoring, Logging, Auditing/Tracking

The [Introduction to the architecture framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro), provides a consistent approach to design cloud solutions by addressing requirements across a pre-defined set of aspects and domains, which are technology-agnostic architectural areas to consider for any enterprise solution. It can be used as a guide to make the necessary design and component choices. After you have identified the applicable requirements and domains that are in scope, you can evaluate and select the best fit for purpose components for your enterprise cloud solution.

In Figure 3, you can view the domains that are relevant in an {{site.data.keyword.satellitelong_notm}} solution.

<!-- use the draw.io framework template to create your heatmap image, located here https://ibm.ent.box.com/file/1389368500379 -->

![Base {{site.data.keyword.satelliteshort}} architecture framework](/images/Base-Satellite-AF.svg){: caption="Figure 3. Base {{site.data.keyword.satellitelong_notm}} Architecture Framework" caption-side="bottom"}


## Solution components and requirements for {{site.data.keyword.satelliteshort}} location on-premises
{: #solution-components-on-prem}

Review the following requirements and components for an on-premises {{site.data.keyword.satelliteshort}} location.  

### Requirements
{: #requirements}

The following table represents a baseline set of requirements, which are applicable to many clients and critical to a successful {{site.data.keyword.satellitelong_notm}} deployment.

| Aspect | Requirement |
|---|---|
| Compute | Customer uses the VMs that are on-premises as the hosts in {{site.data.keyword.satelliteshort}} location. |
| | Customer is looking to use CoreOS |
| Storage | Provide storage that meets the application and database performance requirements. |
| Network | - Provide secure, encrypted connectivity from {{site.data.keyword.satelliteshort}} location to {{site.data.keyword.Bluemix_notm}}. \n - Customer has Direct Link. Use DL to connect {{site.data.keyword.Bluemix_notm}} to hosts in {{site.data.keyword.satelliteshort}} Location. \n - Access customer's existing Red Hat Container Registry.  |
| Data | Data residency requirements require that the customer’s data not leave the region. |
| Security | - Encrypt all application data in transit and at rest to protect it from unauthorized disclosure. \n - Customer would like to use their Hardware Security Module (HSM). \n Note: The HSM owner is responsible for its connectivity, monitoring, and integration with Key Protect. \n - Encrypt all data by using customer-managed keys to meet regulatory compliance requirements for more security and customer control. |
| Resiliency | - Multi-site capability to support a disaster recovery strategy and solution that uses {{site.data.keyword.Bluemix_notm}} infrastructure DR capabilities that are combined with {{site.data.keyword.satelliteshort}} features. \n - Provide backups for data retention. |
| Service management | - Customer wants a fully managed service. \n - Monitor {{site.data.keyword.satelliteshort}} location health metrics to detect issues that might impact availability. \n - Monitor audit logs to track changes. |
| Other | DevOps pipeline to help with deployment of applications to the {{site.data.keyword.satelliteshort}} location |
{: caption="Table 1. Requirements" caption-side="bottom"}

### {{site.data.keyword.satelliteshort}} shared responsibility

{{site.data.keyword.Bluemix_notm}} {{site.data.keyword.satelliteshort}} is a fully managed offering and there are certain responsibilities that are shared by IBM and the customer. The following table explains the breakdown. For more information about the table and the corresponding task details, see [{{site.data.keyword.satelliteshort}} responsibilities](/docs/satellite?topic=satellite-responsibilities).

| Resource | [Incident and operations management](docs/satellite?topic=satellite-responsibilities#incident-and-ops) | [Change management](/docs/satellite?topic=satellite-responsibilities#change-management) | [Identity and access management](/docs/satellite?topic=satellite-responsibilities#iam-responsibilities) | [Security and regulation compliance](/docs/satellite?topic=satellite-responsibilities#security-compliance) | [Disaster Recovery](/docs/satellite?topic=satellite-responsibilities#disaster-recovery) |
|---|---|---|---|---|---|
| Data | Customer | Customer | Customer | Customer | Customer |
| Application | Customer | Customer | Customer | Customer | Customer |
| {{site.data.keyword.satelliteshort}} Location | Shared | Shared | Shared | Shared | Shared |
| {{site.data.keyword.satelliteshort}} Host | Shared | Shared | Shared | Shared | Shared |
| {{site.data.keyword.satelliteshort}} Config | Shared | Shared | Shared | Shared | Shared |
| {{site.data.keyword.satelliteshort}} Link | Shared | Shared | Shared | Shared | Customer |
| {{site.data.keyword.satelliteshort}} Storage	| Shared | Shared | Customer | Shared | Shared |
| {{site.data.keyword.satelliteshort}}-enabled services | Shared | Shared | Shared | Shared | Shared |
| Operating System | Customer | Shared | Customer | Shared | Customer |
| Virtual and bare metal servers | Customer | Customer | Customer | Customer | Customer |
| Virtual storage | Customer | Customer | Customer | Customer | Customer |
| Virtual network | Customer | Customer | Customer | Customer | Customer |
| Hypervisor | Customer | Customer | Customer | Customer | Customer |
| Physical servers and memory | Customer | Customer | Customer | Customer | Customer |
| Physical storage | Customer | Customer | Customer | Customer | Customer |
| Physical network and devices | Customer | Customer | Customer | Customer | Customer |
| Facilities and data centers | Customer | Customer | Customer | Customer | Customer |
{: caption="Table 2. Shared Responsibility Matrix" caption-side="bottom"}

### Components
{: #components}

| Aspect| Component| How the component is used |
|---|---|---|
| Cloud | {{site.data.keyword.satellitelong_notm}} | Distributed cloud paradigm |
| | Location infrastructure | On-premises: Client provided infrastructure |
| | Management plane MZR | Closest region (MZR) to {{site.data.keyword.satelliteshort}} location |
| Compute | {{site.data.keyword.satelliteshort}} location hosts | Virtual machine (VM) or {{site.data.keyword.baremetal_short_sing}} |
| | Host OS | RHEL 8.x or RHCOS |
| | Control plane hosts	| 4 vCPU and 16 GB RAM |
| | {{site.data.keyword.satelliteshort}} services worker nodes hosts: \n Red Hat OpenShift (Customer Workload Cluster) | - 16 vCPU and 64 GB RAM (minimum of 3spares) for Red Hat OpenShift Data Foundation persistent storage. \n - Regular nodes that are tailored to workload but can be as low as 4x16 |
| | {{site.data.keyword.satelliteshort}} services worker nodes hosts : \n Other {{site.data.keyword.satelliteshort}}-enabled services | Based on {{site.data.keyword.satelliteshort}}-enabled service. This reference solution does not include any other services. |
| | Containers | Managed Red Hat OpenShift on {{site.data.keyword.satelliteshort}} |
| | Red Hat OpenShift cluster connectivity | - Private Service cluster URL \n - Public Domain Name System (DNS) pointing to control plane node IPs by default \n - Private {{site.data.keyword.satelliteshort}} link endpoint for Red Hat OpenShift cluster accessible within {{site.data.keyword.Bluemix_notm}} private network  |
| | Workloads Access | - Red Hat OpenShift Routes \n - Node Ports \n - There is the ability to integrate external load balancers, just point load balancer to the Red Hat OpenShift router node port. {: note} |
| | Workload isolation | Single cluster for all workloads |
| | Container Images Registry | {{site.data.keyword.registrylong_notm}} on {{site.data.keyword.Bluemix_notm}} |
| Storage | Primary | |
| | {{site.data.keyword.satelliteshort}} Hosts: Control plane and worker nodes host node local storage
| | {{site.data.keyword.satelliteshort}} Services storage: \n Red Hat OpenShift (Customer Workloads) | Software Defined Storage (SDS) |
| | Software Defined Storage | - Red Hat OpenShift Data Foundation\n - Portworx enterprise (if customer is an existing Portworx user) |
| | Portworx enterprise storage | Worker node host local disks |
| | {{site.data.keyword.satelliteshort}} services storage template: \n Red Hat OpenShift | Bring your Own Driver: Portworx |
| | {{site.data.keyword.satelliteshort}} Services Storage Template: \n Other {{site.data.keyword.satelliteshort}} enabled services | Based on {{site.data.keyword.satelliteshort}} enabled service |
| | Backup | |
| | {{site.data.keyword.satelliteshort}} Control Plane Data | {{site.data.keyword.cos_full_notm}} (IBM-managed backups) |
| | Red Hat OpenShift workload data | Customer might choose to use Cloud Object Storage on {{site.data.keyword.Bluemix_notm}} |
| Networking |Enterprise Connectivity | |
| | {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.dl_full_notm}} 2.0 connect or internet |
| | {{site.data.keyword.satelliteshort}} location private Network | VPN or Use {{site.data.keyword.satelliteshort}} link for Transmission Control Protocol (TCP) and HTTPS connections (no User Datagram Protocol)|
| | Cloud Connectivity | |
| | {{site.data.keyword.satelliteshort}} location connectivity | {{site.data.keyword.satelliteshort}} link over public network
| | {{site.data.keyword.satelliteshort}} Services Connectivity | {{site.data.keyword.satelliteshort}} link location endpoint for Red Hat OpenShift cluster
| | {{site.data.keyword.Bluemix_notm}} services connectivity | {{site.data.keyword.satelliteshort}} link cloud endpoints
| | Load balancers | |
| | Red Hat OpenShift application load balancer | 3rd party load balancer –Ingress Controller |
| | Segmentation | |
| | Red Hat OpenShift cluster | Container network policies |
| | DNS | Client DNS at {{site.data.keyword.satelliteshort}} location |
| Security: Data | Encryption at rest	| |
| | {{site.data.keyword.satelliteshort}} control plane backup storage | Cloud Object Storage encrypted with provider keys |
| | {{site.data.keyword.satelliteshort}} worker nodes data | Worker nodes storage encryption: Customer |
| | Red Hat OpenShift cluster persistent storage | Cluster volume encryption with Kubernetes Secret |
| | Encryption in Transit	| |
| | {{site.data.keyword.satelliteshort}} Link | Encryption that uses TLS |
| | Red Hat OpenShift cluster workloads | App-level encryption that uses TLS |
| | Certificate Lifecycle Management | Customer on-premises certificate manager |
| Security: Identity and Access Management (IAM) | Access and role management	| |
| |	{{site.data.keyword.satelliteshort}} Location | - {{site.data.keyword.Bluemix_notm}} account set up \n - Account and Resource Organization \n - {{site.data.keyword.Bluemix_notm}} IAM roles and access groups |
| | {{site.data.keyword.satelliteshort}} location hosts | {{site.data.keyword.Bluemix_notm}} IAM | |
| | {{site.data.keyword.satelliteshort}} services: \n Red Hat OpenShift (Customer Workloads Cluster) | - {{site.data.keyword.Bluemix_notm}} IAM Roles \n - Kubernetes role-based access control (RBAC) Roles |
| | Application: Runtime security (WAF and DDoS) | Bring your own Edge Security | |
| | Infrastructure and endpoint: Core Network Protection | Subnets and firewall rules | |
| | Threat detection and response: Threat detection | Customer SIEM tool, for example, Splunk | |
| Resiliency | High availability | |
| | {{site.data.keyword.satelliteshort}} Host Nodes (control and worker nodes) | Multi-zone deployment | |
| | Red Hat OpenShift workloads | Multi-zone Red Hat OpenShift cluster | |
| | Resiliency: Backup | |
| | Red Hat OpenShift clusters | Portworx PX Backup for Kubernetes | |
| Service management | Monitoring | |
| | {{site.data.keyword.satelliteshort}} location and hosts | - IBM {{site.data.keyword.satelliteshort}} Monitoring Tool \n - {{site.data.keyword.monitoringlong_notm}} | |
| | Red Hat OpenShift clusters | {{site.data.keyword.monitoringlong_notm}} | |
| | Logging	| |
| | {{site.data.keyword.satelliteshort}} location and hosts | - IBM {{site.data.keyword.satelliteshort}} {{site.data.keyword.loganalysisshort}} tool \n - {{site.data.keyword.loganalysislong}} |
| | Red Hat OpenShift clusters | {{site.data.keyword.loganalysislong_notm}} |
| | Auditing | |
| | {{site.data.keyword.satelliteshort}}e location events | {{site.data.keyword.cloudaccesstraillong}} |
| | Red Hat OpenShift clusters | {{site.data.keyword.cloudaccesstraillong}} |
{: caption="Table 3. Components" caption-side="bottom"}

## Solution components for hybrid {{site.data.keyword.satelliteshort}} locations
{#components-hybrid}

In addition to the components listed in the {{site.data.keyword.satelliteshort}} location on-premises pattern, there are hyperscaler-related components: 

- Hyperscaler infrastructure
- Hyperscaler specific Container Registry
- Direct network connection between {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.Bluemix_notm}}
- Hyperscaler specific activity tracker

The architecture framework is used to guide and determine the applicable aspects and domains for which architecture decisions need to be made. Review the design considerations and architecture decisions for the aspects and domains that are in play in this solution pattern.
---

copyright:
    years: 2024
lastupdated: "2024-1-8"

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


# Base IBM Cloud Satellite <!-- H1 -->
{: #base-ibm-cloud-satellite}
{: toc-content-type="reference-architecture"}
{: toc-use-case="Managed cloud"}
{: toc-version="1.0"}



Two common solution patterns are described here:
1.	One or more Satellite locations configured on-premises
2.	One Satellite location is configured on-premises, and another Satellite location is configured in the cloud


<!-- After the introduction, include a summary of the typical use case for the architecture. The use case might include the motivation for the architecture composition, business challenge, or target cloud environments.
-->

Due to privacy, regulatory or compliance reasons, customers may not have the option to send or store data in the public cloud. In such scenarios, the best option is to create one or more Satellite locations on-premises and host/store data "locally" to satisfy the data residency requirement.

## Architecture diagram <!-- this is an H2 -->
{: #architecture-diagram}



Figure 1 illustrates the IBM Cloud Satellite architecture where one or more Satellite locations are deployed on-premises.


![Satellite location on-premises architecture](/images/SatLoc-on-premises-architecture.svg){: caption="Figure 1. Base IBM Cloud Satellite Solution Architecture with Satellite location on-premises" caption-side="bottom"}



Satellite link connects on-premises Satellite locations to IBM Cloud. Customers may also choose to use Direct Link. Red Hat OpenShift and ODF are two of the many other Satellite-enabled services that are shown deployed in the Satellite location.



Figure 2 illustrates the hybrid IBM Cloud Satellite architecture where one or more Satellite locations are deployed on-premises and the other Satellite location is deployed in another cloud. The figure shows AWS as the hyperscaler.



![Satellite location hybrid architecture](/images/SatLoc-hybrid-architecture.svg){: caption="Figure 2. Base IBM Cloud Satellite Solution Architecture with Hybrid Satellite locations" caption-side="bottom"}

A Satellite location can be created in IBM Cloud, AWS, Azure or Google. In this example one location is shown on AWS, While the other Satellite location is on-premises.

## Design scope <!-- H2 -->
{: #design-scope}

Following the [Architecture Framework](https://test.cloud.ibm.com/docs-draft/architecture-framework?topic=architecture-framework-taxonomy), the base IBM Cloud Satellite solution covers design considerations and architecture decisions for the following aspects and domains:

<!-- include all aspectsand domains relavant to the pattern below -->
- **Compute:** Bare Metal, Virtual Servers, Virtualization, Containers

- **Storage:** Primary Storage, Backup Storage

- **Networking:** Enterprise Connectivity, Network Segmentation

- **Data:** Data Storage (highlighting the Data Residency requirement)

- **Security:** Data Security, Identity and Access Management, Infrastructure and Endpoint

- **Resiliency:** Backup and Restore, High Availability

- **Service Management:** Monitoring, Logging, Auditing/Tracking

The Architecture Framework, described in [Introduction to the Architecture Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro), provides a consistent approach to design cloud solutions by addressing requirements across a pre-defined set of aspects and domains, which are technology-agnostic architectural areas that need to be considered for any enterprise solution. It can be used as a guide to make the necessary design and component choices to ensure you have considered applicable requirements for each aspect and domain. After you have identified the applicable requirements and domains that are in scope, you can evaluate and select the best fit for purpose components for your enterprise cloud solution.

The Figure 3 shows the domains that are relevant in an IBM Cloud Satellite solution.

<!-- use the draw.io framework template to create your heatmap image, located here https://ibm.ent.box.com/file/1389368500379 -->

![Base Satellite architecture framework](/images/Base-Satellite-AF.svg){: caption="Figure 3. Base IBM Cloud Satellite Architecture Framework" caption-side="bottom"}

A. Solution Components for Satellite location on-premises
---

## Requirements <!-- H2 -->
{: #requirements}

<!-- insert the requirements table, below is an example -->

The following represents a baseline set of requirements which are applicable to many clients and critical to successful IBM Cloud Satellite deployment.

| Aspect | Requirement |
|---|---|
| Compute | Customer would like to use the VMs they have on-premises as hosts in Satellite location. |
| | Customer is looking to use CoreOS |
| Storage | Provide storage that meets the application and database performance requirements. |
| Network | Provide secure, encrypted connectivity from Satellite location to IBM Cloud. |
| | Customer has Direct Link. Use DL to connect IBM Cloud to hosts in Satellite Location. |
| | Access customer's existing Red Hat Container Registry. |
| Data | Data residency requirements require that the customer’s data not leave the region. |
| Security | Encrypt all application data in transit and at rest to protect from unauthorized disclosure. |
| | Customer would like to use their Hardware Security Module (HSM). \n  Note: HSM owner is responsible for its connectivity, monitoring, and integration with Key Protect |
| | Encrypt all data using customer managed keys to meet regulatory compliance requirements for additional security and customer control. |
| Resiliency | Multi-site capability to support a disaster recovery strategy and solution leveraging IBM Cloud infrastructure DR capabilities combined with Satellite features |
| | Provide backups for data retention |
| Service Management | Customer wants a fully managed service. |
| | Monitor Satellite location health metrics to detect issues that might impact availability. |
| | Monitor audit logs to track changes. |
| Other | DevOps pipeline to help with deployment of applications to the Satellite location |
{: caption="Table 1. Requirements" caption-side="bottom"}

## Satellite Shared Responsibility Matrix <!-- H2 -->

IBM Cloud Satellite is a fully managed offering hence there are certain responsibilities that are shared by IBM and the customer. The table below shows the breakdown. The table and the corresponding task details can  be found [here](https://cloud.ibm.com/docs/satellite?topic=satellite-responsibilities).

| Resource | [Incident & operations management](https://cloud.ibm.com/docs/satellite?topic=satellite-responsibilities#incident-and-ops) | [Change management](https://cloud.ibm.com/docs/satellite?topic=satellite-responsibilities#change-management) | [Identity & access management](https://cloud.ibm.com/docs/satellite?topic=satellite-responsibilities#iam-responsibilities) | [Security &regulation compliance](https://cloud.ibm.com/docs/satellite?topic=satellite-responsibilities#security-compliance) | [Disaster Recovery](https://cloud.ibm.com/docs/satellite?topic=satellite-responsibilities#disaster-recovery) |
|---|---|---|---|---|---|
| Data | Customer | Customer | Customer | Customer | Customer |
| Application | Customer | Customer | Customer | Customer | Customer |
| Satellite Location | Shared | Shared | Shared | Shared | Shared |
| Satellite Host | Shared | Shared | Shared | Shared | Shared |
| Satellite Config | Shared | Shared | Shared | Shared | Shared |
| Satellite Link | Shared | Shared | Shared | Shared | Customer |
| Satellite Storage	| Shared | Shared | Customer | Shared | Shared |
| Satellite-enabled services | Shared | Shared | Shared | Shared | Shared |
| Operating System | Customer | Shared | Customer | Shared | Customer |
| Virtual & bare metal servers | Customer | Customer | Customer | Customer | Customer |
| Virtual storage | Customer | Customer | Customer | Customer | Customer |
| Virtual network | Customer | Customer | Customer | Customer | Customer |
| Hypervisor | Customer | Customer | Customer | Customer | Customer |
| Physical servers & memory | Customer | Customer | Customer | Customer | Customer |
| Physical storage | Customer | Customer | Customer | Customer | Customer |
| Physical network & devices | Customer | Customer | Customer | Customer | Customer |
| Facilities and data centers | Customer | Customer | Customer | Customer | Customer |
{: caption="Table 2. Shared Responsibility Matrix" caption-side="bottom"}

## Components <!-- H2 -->
{: #components}

<!-- insert the components list, below is an example -->

| Aspect| Component| How the component is used |
|---|---|---|
| Cloud | IBM Cloud Satellite | Distributed Cloud paradigm |
| | Satellite: Location Infrastructure | On-premises: Client provided infrastructure |
| | Satellite: Management Plane MZR | Closest region (MZR) to Satellite Location |
| Compute | Satellite Location Hosts | Virtual Machine (VM) or Bare Metal |
| | Host OS | RHEL 8.x or RHCOS |
| | Control Plane Hosts	| 4 vCPU and 16GB RAM |
| | Satellite Services Worker Nodes Hosts: \n  OpenShift (Customer Workload Cluster) | 16 vCPU and 64GB RAM (min 3+spares) for ODF persistent storage. \n  Regular nodes tailored to workload but can be as low as  4x16 |
| | Satellite Services Worker Nodes Hosts: \n  Other Satellite-Enabled-Services | Based on Satellite-enabled service. This reference solution does not include any other services. |
| | Containers | Managed OpenShift on Satellite |
| | OpenShift Cluster Connectivity | Private Service Cluster URL \n  Public DNS pointing to control plane node IPs by default OR \n  Private satellite link endpoint for openshift cluster accessible within IBM Cloud private network  |
| | Workloads Access | OpenShift Routes \n  Node Ports \n  Note: there is the ability to integrate external load balancers (basically just point load balancer to the openshift router node port) |
| | Workload Isolation | Single cluster for all workloads |
| | Container Images Registry | IBM Container Registry on IBM Cloud |
| Storage | Storage: Primary | |
| | Satellite Hosts: Control Plane & Worker Nodes	Host node local storage
| | Satellite Services Storage: \n  OpenShift (Customer Workloads) | Software Defined Storage (SDS) |
| | Software Defined Storage | ODF \n  Portworx Enterprise (if customer is existing Portworx user) |
| | Portworx Enterprise Storage | Worker Node Host local disks |
| | Satellite Services Storage Template: \n  OpenShift | Bring your Own Driver – Portworx |
| | Satellite Services Storage Template: \n  Other Satellite-Enabled Services | Based on Satellite Enabled Service |
| | Storage: Backup | |
| | Satellite Control Plane Data | IBM Cloud Object Storage (IBM-managed backups) |
| | OpenShift Workload Data | Customer may choose to use Cloud Object Storage (COS) on IBM Cloud |
| Networking | Networking: Enterprise Connectivity | |
| | Satellite Location and IBM Cloud | Direct Link 2.0 Connect or Internet |
| | Satellite Location Private Network | VPN or Use satellite link for TCP/HTTPS connections (no UDP)|
| | Networking: Cloud Connectivity | |
| | Satellite Location Connectivity | Satellite Link over Public Network
| | Satellite Services Connectivity | Satellite Link Location Endpoint for OpenShift Cluster
| | IBM Cloud Services Connectivity | Satellite Link Cloud Endpoints
| | Networking: Load Balancers | |
| | OpenShift Application Load Balancer | 3rd party load balancer –Ingress Controller |
| | Networking: Segmentation | |
| | OpenShift Cluster | Container Network Policies |
| | Networking: DNS | Client DNS at Satellite location |
| Security: Data | Data: Encryption at Rest	| |
| | Satellite Control Plane Backup Storage | COS encrypted with provider keys |
| | Satellite Worker Nodes Data | Worker Nodes Storage Encryption - Customer |
| | OpenShift Cluster Persistent Storage | Cluster Volume Encryption with Kubernetes Secret |
| | Data: Encryption in Transit	| |
| | Satellite Link | Encryption using TLS |
| | OpenShift Cluster Workloads | App-level encryption using TLS |
| | Data: Certificate Lifecycle Management | Customer on-prem certificate manager |
| Security: Identity and Access Management (IAM) | Identify and Access: Access & Role Management	| |
| |	Satellite Location | IBM Cloud Account Setup \n  Account & Resource Organization \n  IBM Cloud IAM Roles/Access Groups |
| | Satellite Location Hosts | IBM Cloud IAM | |
| | Satellite Services: \n  OpenShift (Customer Workloads Cluster) | IBM Cloud IAM Roles \n  Kubernetes Role-based access control (RBAC) Roles |
| | Application: Runtime Security (WAF & DDoS) | Bring your own Edge Security | |
| | Infrastructure & Endpoint: Core Network Protection | Subnets, firewall rules | |
| | Threat Detection & Response: Threat Detection | Customer SIEM tool (e.g. Splunk) | |
| Resiliency | Resiliency: High Availability | |
| | Satellite Host Nodes (control and worker nodes) | Multi-zone deployment | |
| | OpenShift Workloads | Multi-zone OpenShift Cluster | |
| | Resiliency: Backup | |
| | OpenShift Clusters | Portworx PX Backup for Kubernetes | |
| Service Management | Service Management: Monitoring | |
| | Satellite Location & Hosts | IBM Satellite Monitoring Tool \n  IBM Cloud Monitoring | |
| | OpenShift Clusters | IBM Cloud Monitoring | |
| | Service Management: Logging	| |
| | Satellite Location & Hosts | IBM Satellite Log Analysis Tool \n  IBM Log Analysis |
| | OpenShift Clusters | IBM Log Analysis |
| | Service Management: Auditing | |
| | Satellite Location Events | IBM Cloud Activity Tracker |
| | OpenShift Clusters | IBM Cloud Activity Tracker |
{: caption="Table 3. Components" caption-side="bottom"}

B. Solution Components for hybrid Satellite locations
---

In addition to the components listed in the Satellite location on-premises pattern, above, there would be hyperscaler related components. For example:
- Hyperscaler infrastructure
- Hyperscaler specific container registry
- Direct network connection between Satellite location and IBM Cloud
- Hperscaler specific activity tracker

The Architecture Framework is used to guide and determine the applicable aspects and domains for which architecture decisions need to be made. Access the design considerations and architecture decisions for the aspects and domains that are in play in this solution pattern, from the left navigation menu.

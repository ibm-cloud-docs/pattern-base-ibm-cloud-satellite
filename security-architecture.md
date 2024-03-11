---

copyright:
  years: 2024
lastupdated: "2024-02-12"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

# Architecture decisions for security
{: #security-architecture}

The following are security architecture decisions for the deployment of Satellite on-premises or in hyperscaler pattern.

## Architecture decisions for data encryption
{: #data-encryption}

| Architecture decision | Requirement |  Option | Decision | Rationale |
|---|---|---|---|---|
| Data encryption | - Encryption at rest \n - {{site.data.keyword.satelliteshort}} worker nodes data	| Worker nodes storage encryption: Customer | Worker nodes storage encryption: Customer	| The customer is responsible for encrypting the boot disk and any additional disks that you add to the {{site.data.keyword.satelliteshort}} worker nodes hosts to keep data secure and meet regulatory requirements. |
| | Encryption at rest \n Red Hat OpenShift persistent storage | - Backing-storage device encryption \n Cluster volume encryption with Kubernetes Secret \n {{site.data.keyword.keymanagementservicelong}} \n Bring your own (BYO) Hardware Security Module (HSM) | Cluster volume encryption with Kubernetes Secret | - The customer is responsible for encrypting persistent storage by configuring storage device encryption or cluster encryption, if supported by the storage provider.\n In this solution, Portworx is used to provide persistent storage for Red Hat OpenShift cluster workloads. \n - Example Portworx set up: volume encryption with customers keys stored in Kubernetes Secret to encrypt data in transit and at rest. \n - Add Kubernetes Secret encryption |
| | Encryption at rest and in transit \n Backup storage	| - {{site.data.keyword.cos_full_notm}} encrypted with provider keys \n {{site.data.keyword.cos_full_notm}} encrypted with Key Management Service (KMS) | {{site.data.keyword.cos_full_notm}} encrypted with provider keys	|  {{site.data.keyword.IBM_notm}}-managed backups of the {{site.data.keyword.satelliteshort}} location control plane data are stored in customer created {{site.data.keyword.cos_full_notm}} buckets. Customer can select to encrypt {{site.data.keyword.cos_full_notm}} bucket with {{site.data.keyword.Bluemix_notm}} keys or KMS (Key Protect or Hyper Protect Crypto Services) created in {{site.data.keyword.Bluemix_notm}} MZR used to manage {{site.data.keyword.satelliteshort}}. In this solution, the customer {{site.data.keyword.cos_full_notm}} bucket is encrypted with {{site.data.keyword.Bluemix_notm}} keys. |
| | Encryption in transit \n {{site.data.keyword.satelliteshort}} link	| {{site.data.keyword.satelliteshort}} link encryption	| {{site.data.keyword.satelliteshort}} Link encryption	| All data that is transported over {{site.data.keyword.satelliteshort}} link is encrypted by using TLS 1.3 standards. This level of encryption is managed by {{site.data.keyword.IBM_notm}}. |
| | Encryption in transit \n Red Hat OpenShift cluster workloads | App level encryption that uses TLS \n Red Hat OpenShift service mesh | App-level encryption that uses TLS | Encryption in transit of application data is the customer’s responsibility. Applications can encrypt data by using TLS 1.2 at a minimum. |
| Certificates | Certificate lifecycle management | - Secrets Manager on {{site.data.keyword.Bluemix_notm}} \n Bring your own certificates | Bring your own certificates | The customer is responsible for providing and managing TLS certificates that are used for encrypting communication for workloads that are deployed on {{site.data.keyword.satelliteshort}} clusters. |
{: caption="Table 1. Data encryption architecture decisions" caption-side="bottom"}


## Architecture decisions for identity and access management
{: #iam}

| Architecture decision | Requirement |  Option | Decision | Rationale |
|---|---|---|---|---|
| {{site.data.keyword.iamlong}} (IAM) | {{site.data.keyword.satelliteshort}} location hosts | {{site.data.keyword.iamshort}} | {{site.data.keyword.iamshort}}  | After a host is assigned to a {{site.data.keyword.satelliteshort}} location, root SSH access is disabled (per CIS benchmark) and access to the host is controlled by [IAM access](/docs/openshift?topic=openshift-users) |
|  | {{site.data.keyword.satelliteshort}} location | - {{site.data.keyword.Bluemix_notm}} account setup \n Account and resource organization \n {{site.data.keyword.Bluemix_notm}} IAM roles and access groups | - {{site.data.keyword.Bluemix_notm}} account setup \n Account and resource organization \n {{site.data.keyword.Bluemix_notm}} IAM roles and access groups | Account structure and access management with IAM RBAC enables zero trust through separation of duty and least privileged access. \n For more information, see [Account and resource organization](/docs/satellite?topic=satellite-iam-assign-access) and [{{site.data.keyword.Bluemix_notm}} IAM roles and access groups](/docs/satellite?topic=satellite-iam-platform-access). \n [{{site.data.keyword.satellitelong_notm}} platform and service access roles](/docs/satellite?topic=satellite-iam) in IAM are used to authenticate requests to the service and authorize user actions. |
|  | Red Hat OpenShift clusters | - [IAM roles](/docs/openshift?topic=openshift-users) federated with a customer active directory \n [Kubernetes RBAC Roles](/docs/openshift?topic=openshift-users) | - {{site.data.keyword.Bluemix_notm}} IAM roles \n Kubernetes role-based access control (RBAC) roles | - Red Hat OpenShift on {{site.data.keyword.Bluemix_notm}} uses IAM [platform and service access roles](/docs/openshift?topic=openshift-users) to grant users access to the cluster \n - RBAC roles and cluster roles define a set of permissions for how users can interact with Kubernetes resources in the cluster. - RBAC roles can be applied to individual users, groups of users, or service accounts. For more granular access policies to perform specific Kubernetes actions, you can apply [custom RBAC policies](/docs/openshift?topic=openshift-users). |
{: caption="Table 2. Identity and access management architecture decisions" caption-side="bottom"}

## Architecture decisions for application security
{: #app-security}

| Architecture decision | Requirement |  Option | Decision | Rationale |
|---|---|---|---|---|
| Application security | Enforce runtime security to protect against distributed denial-of-service (DDOS) attacks. | Bring your own edge security | Bring your own edge security | The customer is responsible for providing edge solution at {{site.data.keyword.satelliteshort}} location to protect applications that are exposed to the public network. |
{: caption="Table 3. Application security architecture decisions" caption-side="bottom"}

Edge security generally protects against attacks and creates secure connections. It includes intrusion detection and prevention, URL and domain filtering, secure web gateway, zero trust network access (ZTNA), and other technologies, which help in isolating the location.

## Architecture decisions for infrastructure and endpoint
{: #infrastructure and endpoint}

| Architecture decision | Requirement |  Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Network protection | Core network protection |  Subnets and firewall rules | Subnets and firewall rules | The customer is responsible for setting up and managing physical and virtual networks, subnets, and firewalls rules at the {{site.data.keyword.satelliteshort}} location to meet security and regulatory requirements. |
{: caption="Table 4. Infrastructure and endpoint architecture decisions" caption-side="bottom"}

## Architecture decisions for threat detection and response
{: #threat detection and response}

| Architecture decision | Requirement |  Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Threat detection and response (TDR) |  Identify and neutralize threats | Bring your own security information and event management (SIEM) tool, for example, Splunk. \n [IBM X-Force Threat Management](https://www.ibm.com/products/xforce-exchange) | Bring your own SIEM tool, for example, Splunk. | For hybrid cloud environments, customers typically prefer to use their current on-premises SIEM tools. |
{: caption="Table 5. Threat detection and response architecture decisions" caption-side="bottom"}

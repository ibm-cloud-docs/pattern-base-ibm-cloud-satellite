---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

# Architecture decisions for security
{: #security-architecture}

<!-- below is a placeholder for all compute domain decisions  Remove the domains that are not in scope.  If there are decisions
that need to be added (e.g. platform dependent) add additional rows-->

The following are security architecture decisions for the base IBM Cloud Satellite pattern.

## Architecture decisions for data security - encryption
{: #data-encryption}

| Architecture decision | Requirement |  Option | Decision | Rationale |
|---|---|---|---|---|
| Data Encryption | Data: Encryption at Rest \n  Satellite Worker Nodes Data	| Worker Nodes Storage Encryption - Customer | Worker Nodes Storage Encryption - Customer	| Customer is responsible for encrypting the boot disk and any additional disks added to the Satellite worker nodes hosts to keep data secure and meet regulatory requirements. |
| | Data: Encryption at Rest \n  OpenShift Persistent Storage | - Backing-storage Device Encryption \n- Cluster Volume Encryption with Kubernetes Secret \n- Key Protect on IBM Cloud \n- Bring your own (BYO) HSM | Cluster Volume Encryption with Kubernetes Secret | The customer is responsible for encrypting persistent storage by configuring storage device encryption or cluster encryption, if supported by the storage provider. In this solution, Portworx is used to provide persistent storage for OpenShift cluster workloads. \n  Example Portworx set up: volume encryption with customers keys stored in Kubernetes Secret to encrypt data in transit and at rest. \n  add Kubernetes Secret encryption |
| | Data: Encryption at Rest & in Transit \n  Backup Storage	| - COS encrypted w/ provider keys \n- COS encrypted with Key Management Service (KMS) | COS encrypted w/ provider keys	| IBM-managed backups of the Satellite location control plane data are stored in customer-provisioned COS buckets. Customer can select to encrypt COS bucket with IBM Cloud keys or KMS (Key Protect or HPCS) provisioned in IBM Cloud MZR used to manage Satellite. In this solution, the customer COS bucket is encrypted with IBM Cloud keys. |
| | Data: Encryption in Transit \n  Satellite Link	| Satellite Link encryption	| Satellite Link encryption	| All data that is transported over Satellite Link is encrypted using TLS 1.3 standards. This level of encryption is managed by IBM. |
| | Data: Encryption in Transit \n  OpenShift Cluster Workloads | App-level encryption using TLS \n  OpenShift Service Mesh | App-level encryption using TLS | Encryption in transit of application data is the customer’s responsibility. Applications can encrypt data using TLS 1.2 at a minimum. |
| Certificates | Data: Certificate Lifecycle Management | - Secrets Manager on IBM Cloud \n- BYO Certificates | BYO Certificates | Customer is responsible for providing and managing TLS certificates used for encrypting communication for workloads deployed on Satellite clusters. |
{: caption="Table 1. Data encryption architecture decisions" caption-side="bottom"}


## Architecture decisions for identity and access management
{: #iam}

| Architecture decision | Requirement |  Option | Decision | Rationale |
|---|---|---|---|---|
| Identity and access management (IAM) | Identity Access & Role Management: Satellite Location Hosts | IBM Cloud IAM | IBM Cloud IAM  | After a host is assigned to a Satellite Location, root SSH access is disabled (per CIS benchmark) and access to the host is controlled via [IBM Cloud IAM access](https://cloud.ibm.com/docs/openshift?topic=openshift-users) |
|  | Identity Access & Role Management: Satellite Location | - IBM Cloud Account Setup \n- Account & Resource Organization \n- IBM Cloud IAM Roles/Access Groups | - IBM Cloud Account Setup \n- Account & Resource Organization \n- IBM Cloud IAM Roles/Access Groups | Account structure and access management with IAM RBAC enables Zero Trust through Separation of Duty and Least Privileged Access. \n  See [Account and Resource Organization](https://test.cloud.ibm.com/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-account-organizationhttps://test.cloud.ibm.com/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-account-organization) and [IBM Cloud IAM Roles / Access Groups](https://test.cloud.ibm.com/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-account-organization). \n  [IBM Cloud Satellite platform and service access roles](https://cloud.ibm.com/docs/satellite?topic=satellite-iam) in Identity and Access Management (IAM) are used to authenticate requests to the service and authorize user actions. |
|  | Identity Access & Role Management: OpenShift Clusters | - [IBM Cloud IAM roles](https://cloud.ibm.com/docs/openshift?topic=openshift-users) \n-  IAM Roles federated w/ customer AD \n-  [Kubernetes RBAC Roles](https://cloud.ibm.com/docs/openshift?topic=openshift-users) | - IBM Cloud IAM roles \n-  Kubernetes RBAC Roles | - OpenShift on IBM Cloud uses IBM Cloud IAM [platform and service access roles](https://cloud.ibm.com/docs/openshift?topic=openshift-users) to grant users access to the cluster \n- RBAC roles and cluster roles define a set of permissions for how users can interact with Kubernetes resources in the cluster. RBAC roles can be applied to individual users, groups of users, or service accounts \n- For more granular access policies to perform select Kubernetes actions use [custom RBAC policies](https://cloud.ibm.com/docs/openshift?topic=openshift-users) |
{: caption="Table 2. Identity and access management architecture decisions" caption-side="bottom"}

## Architecture decisions for application security
{: #app-security}

| Architecture decision | Requirement |  Option | Decision | Rationale |
|---|---|---|---|---|
| Application security | Enforce runtime security (Protect against distributed denial-of-service (DDOS) attacks.) | Bring your own Edge Security | Bring your own Edge Security | Customer is responsible for providing edge solution at Satellite Location to protect applications exposed to the public network |
{: caption="Table 3. Application security architecture decisions" caption-side="bottom"}

## Architecture decisions for infrastructure and endpoint
{: #infrastructure and endpoint}

| Architecture decision | Requirement |  Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Network protection | Core Network Protection |  Subnets, firewall rules | Subnets, firewall rules | Customer is responsible for setting up and managing physical and virtual networks, subnets, and firewalls rules at the Satellite Location to meet security and regulatory requirements. |
{: caption="Table 4. Infrastructure and endpoint architecture decisions" caption-side="bottom"}

## Architecture decisions for threat detection and response
{: #threat detection and response}

| Architecture decision | Requirement |  Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Threat detection and response (TDR) |  Identify and neutralize threats | - BYO Security information and event management (SIEM) tool (e.g.  Splunk) \n- [IBM X-Force Threat Management](https://www.ibm.com/products/xforce-exchange) | BYO SIEM tool (e.g. Splunk) | For hybrid cloud environments, customers typically prefer to use their current on-prem SIEM tools. |
{: caption="Table 5. Threat detection and response architecture decisions" caption-side="bottom"}

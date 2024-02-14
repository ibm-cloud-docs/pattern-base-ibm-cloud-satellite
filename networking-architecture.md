---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

<!-- Below is a placeholder for all compute domain decisions.  Remove the domains that are not in scope.  If there are decisions
that need to be added (e.g. platform dependent) add additional rows-->

The following tables summarize the networking architecture decisions for the {{site.data.keyword.satellitelong_notm}} base pattern.

## Architecture decisions for enterprise connectivity
{: #enterprise-connectivity}

The following are architecture decisions for enterprise connectivity for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| | Connectivity between Satellite Location and IBM Cloud | - Satellite Link \n- Public Network \n- Direct Link | Satellite Link over Public Network | Satellite Link proxies network traffic over a secure TLS connection between cloud services and resources in the Satellite location. This link tunnel serves as a communication path over the internet that uses the Transmission Control Protocol (TCP) protocol and port 443. This is the default configuration. |
| | Satellite Location using Private Network | VPN | VPN | VPN connection to the Satellite Location private network is needed to access Satellite Location resources/services, e.g. OpenShift Console, not exposed to the public network. |
| | Cloud Connectivity to Satellite Enabled Services \n  OpenShift (Customer Workloads) | Satellite Link Location Endpoint | Satellite Link Location Endpoint | Satellite Link Location Endpoints provide access to Satellite Location resources/services from within the IBM Cloud private network. \n  By default, IBM Satellite creates a Satellite Link Location Endpoint to access the OpenShift Cluster running in the location. This endpoint can be optionally enabled to allow the cluster to be managed through Satellite Config. |
{: caption="Table 1. Architecture decisions for enterprise connectivity" caption-side="bottom"}



## Architecture decisions for network segmentation and isolation
{: #network-segmentation-isolation}

The following are network segmentation and isolation architecture decisions for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| | Network segmentation and isolation | - [Container Network Policies](https://cloud.ibm.com/docs/openshift?topic=openshift-network_policies) \n- OpenShift Service Mesh | Container Network Policies | Container network policies restrict egress/ingress traffic and communication between applications. \n- Allow or block network traffic on specific network interfaces regardless of the Kubernetes pod source or destination IP address or CIDR. \n- Allow or block network traffic for pods across namespaces. For additional guidance see [link](https://cloud.ibm.com/docs/openshift?topic=openshift-vpc-network-policy). |
{: caption="Table 2. Architecture decisions for network segmentation and isolation" caption-side="bottom"}



## Architecture decisions for load balancing
{: #network-load-balancing}

The following are load balancing architecture decisions for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| | Application Load Balancer (ALB) | - 3rd party Load Balancer –Ingress Controller \n- External LB in Cloud Provider | 3rd party Load Balancer –Ingress Controller  | Use a [third-party load balancer and OpenShift routes](https://cloud.ibm.com/docs/openshift?topic=openshift-sat-expose-apps) to expose apps with a hostname and add health checking for the host IP addresses that are registered in the router's Domain Name System (DNS)records. As an example, [MetalLB](https://metallb.universe.tf/) can be deployed on OpenShift Cluster Worker Nodes dedicated to the Ingress Controller. |
{: caption="Table 3. Architecture decisions for load balancing" caption-side="bottom"}



## Architecture decisions for domain name system
{: #dns}

The following are DNS architecture decisions for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| | Public DNS | - Client DNS at Satellite location \n- Cloud Internet Services | Client DNS at Satellite location | Provides consistent DNS across Satellite location private cloud to support custom DNS domains (vs DNS domains provided by default) |
{: caption="Table 4. Architecture decisions for domain name system" caption-side="bottom"}

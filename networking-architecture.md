---

copyright:
  years: 2024
lastupdated: "2024-02-20"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following tables summarize the networking architecture decisions for the deployment of Satellite on-premises or in hyperscaler pattern.

## Architecture decisions for enterprise connectivity
{: #enterprise-connectivity}

The following are architecture decisions for enterprise connectivity for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| Network connectivity | Connectivity between {{site.data.keyword.satelliteshort}} Location and {{site.data.keyword.Bluemix_notm}}  | - {{site.data.keyword.satelliteshort}} link \n - Public network \n - Direct link | {{site.data.keyword.satelliteshort}} link over public network | {{site.data.keyword.satelliteshort}} link proxies network traffic over a secure TLS connection between cloud services and resources in the {{site.data.keyword.satelliteshort}} location. This link tunnel serves as a communication path over the internet that uses the Transmission Control Protocol (TCP) and port 443. This is the default configuration. |
| | {{site.data.keyword.satelliteshort}} location that uses private network | Virtual Private Network (VPN) | VPN | VPN connection to the {{site.data.keyword.satelliteshort}} location private network is needed to access {{site.data.keyword.satelliteshort}} location resources and services, for example, the Red Hat OpenShift console that's not exposed to the public network. |
| | Cloud connectivity to {{site.data.keyword.satelliteshort}} enabled services \n Red Hat OpenShift (Customer workloads) | {{site.data.keyword.satelliteshort}} link location endpoint | {{site.data.keyword.satelliteshort}} link location endpoint | {{site.data.keyword.satelliteshort}} link Location endpoints provide access to {{site.data.keyword.satelliteshort}} location resources and services from within the {{site.data.keyword.Bluemix_notm}} private network. \n By default, {{site.data.keyword.satellitelong_notm}} creates a {{site.data.keyword.satelliteshort}} link location endpoint to access the Red Hat OpenShift cluster that runs in the location. This endpoint can be optionally enabled to allow the cluster to be managed through a {{site.data.keyword.satelliteshort}} configuration. |
{: caption="Architecture decisions for enterprise connectivity" caption-side="bottom"}



## Architecture decisions for network segmentation and isolation
{: #network-segmentation-isolation}

The following are network segmentation and isolation architecture decisions for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| Network segmentation | Network segmentation and isolation | - [Container network policies](/docs/openshift?topic=openshift-network_policies) \n - Red Hat OpenShift service mesh | Container network policies | Container network policies restrict egress and ingress traffic and communication between applications. \n - Allow or block network traffic on specific network interfaces regardless of the Kubernetes pod source or destination IP address or Classless Inter-Domain Routing (CIDR). \n - Allow or block network traffic for pods across namespaces. For more information, see [Overview of network security options](/docs/openshift?topic=openshift-vpc-network-policy). |
{: caption="Architecture decisions for network segmentation and isolation" caption-side="bottom"}



## Architecture decisions for load balancing
{: #network-load-balancing}

The following are load balancing architecture decisions for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
|Load Balancing | Application Load Balancer (ALB) | - 3rd party load balancer: Ingress controller \n - External load balancer in cloud provider | 3rd party load balancer: Ingress controller  | Use a [third-party load balancer and Red Hat OpenShift routes](docs/openshift?topic=openshift-sat-expose-apps) to expose apps with a hostname and add health checking for the host IP addresses that are registered in the router's Domain Name System (DNS) records. As an example, [MetalLB](https://metallb.universe.tf/){: external} can be deployed on Red Hat OpenShift cluster worker nodes that are dedicated to the Ingress controller. |
{: caption="Architecture decisions for load balancing" caption-side="bottom"}



## Architecture decisions for domain name system
{: #dns}

The following are DNS architecture decisions for this design.

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| Domain Name System (DNS) | Public DNS | - Client DNS at {{site.data.keyword.satelliteshort}} location \n -  Cloud Internet Services (CIS) | Client DNS at {{site.data.keyword.satelliteshort}} location | Provides consistent DNS across {{site.data.keyword.satelliteshort}} location private cloud to support custom DNS domains instead of DNS domains provided by default. |
{: caption="Architecture decisions for domain name system" caption-side="bottom"}

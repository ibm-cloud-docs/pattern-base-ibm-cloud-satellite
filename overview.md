---

copyright:
  years: 2024
lastupdated: "2024-02-08"

subcollection: pattern-base-ibm-cloud-satellite

keywords: Satellite, location

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

<!-- Note to author>    THIS SHOULD BE ABOUT 10 – 15 LINES AND FOLLOW….
The objective of this pattern is to provide a solution design for……. -->

The objective of this document is to provide a solution design for the deployment of {{site.data.keyword.satellitelong}} with one or more {{site.data.keyword.satelliteshort}} locations. {{site.data.keyword.satelliteshort}} is a software platform that creates managed cloud services across any environment. 

In this guide, you can find the useful information about the configuration of a {{site.data.keyword.satelliteshort}} location that can be either on-premises, in {{site.data.keyword.Bluemix_short}}, or within any other cloud.

The pattern follows the {{site.data.keyword.IBM}} architecture framework and provides a solution design from the standard {{site.data.keyword.Bluemix_notm}} deployment architecture for these two facets of a solution:

- Managed from: The managed from environment is {{site.data.keyword.Bluemix_notm}}
- Managed to: The managed to environment is the {{site.data.keyword.satelliteshort}} location.

In addition, you can find a prescriptive, end to end enterprise-class solution design, with diagrams and component architecture decisions for each of the three {{site.data.keyword.satelliteshort}} locations.

While the document describes the various aspects of a {{site.data.keyword.satelliteshort}} solution, network connectivity and security are two key aspects that customers pay close attention to. The objective of this pattern document is to serve as a guide to meet typical customer requirements and provide a base reference solution for a distributed cloud solution that is secure and resilient.

## {{site.data.keyword.satelliteshort}} components
{: sat-components}

There are two major components in {{site.data.keyword.satelliteshort}}:
- The management plane components along with the services that are available on {{site.data.keyword.Bluemix_notm}}.
- The {{site.data.keyword.satelliteshort}}-enabled services available at a {{site.data.keyword.satelliteshort}} location.

Enterprise-class, mission-critical workloads need to be resilient with high availability (HA) and provide disaster recovery (DR) capabilities. While this base {{site.data.keyword.satelliteshort}} pattern illustrates one on-premises {{site.data.keyword.satelliteshort}} location, there can be multiple {{site.data.keyword.satelliteshort}} locations depending on the requirements and viability of a particular solution. Another scenario that is shown is one on-premises {{site.data.keyword.satelliteshort}} location and another location in a hyperscaler cloud.

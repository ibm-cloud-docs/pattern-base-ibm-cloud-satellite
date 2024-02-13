---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: pattern-base-ibm-cloud-satellite

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design <!-- H1 -->
{: #compute-design}

<!-- text for compute design considerations goes here -->

IBM Cloud {{site.data.keyword.Satellite}} offers flexible hardware configurations. For the {{site.data.keyword.Satellite}} location, customers can bring their own on-premises hardware or purchase from IBM or IBM partners or use their AWS, Azure or Google Cloud infrastructure.
- [{{site.data.keyword.Satellite}} locations](https://cloud.ibm.com/docs/satellite?topic=satellite-location-host) are made up of compute sources, called hosts, from separate zones of your infrastructure environment. The {{site.data.keyword.Satellite}} host i.e. the compute source is attached to a {{site.data.keyword.Satellite}} location, and is assigned to the location control plane or to a {{site.data.keyword.Satellite}}-enabled IBM Cloud service to provide the computing power to run the service workloads. The type of service will determine the kind of compute resources needed to run the service at the location.
- The IBM Cloud service that is set up in a {{site.data.keyword.Satellite}} location is managed from the IBM Cloud region. IBM provides all the resources needed to manage the locations.

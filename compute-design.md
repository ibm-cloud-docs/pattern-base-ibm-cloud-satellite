---

copyright:
  years: 2024
lastupdated: "2024-02-14"

subcollection: pattern-base-ibm-cloud-satellite

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}


{{site.data.keyword.Bluemix_notm}} {{site.data.keyword.satelliteshort}} offers flexible hardware configurations. For the {{site.data.keyword.satelliteshort}} location, customers can bring their own on-premises hardware or purchase from {{site.data.keyword.IBM}} or {{site.data.keyword.IBM_notm}} partners or use their AWS, Azure, or Google Cloud infrastructure.

- [{{site.data.keyword.satelliteshort}} locations](https://cloud.ibm.com/docs/satellite?topic=satellite-location-host) are made up of compute sources, called hosts, from separate zones of your infrastructure environment. The {{site.data.keyword.satelliteshort}} host is attached to a {{site.data.keyword.satelliteshort}} location, and is assigned to the location control plane or to a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.Bluemix_notm}} service to provide the computing power to run the service workloads. The type of service determines the compute resources that are needed to run the service at the location.

- The {{site.data.keyword.Bluemix_notm}} service that is set up in a {{site.data.keyword.satelliteshort}} location is managed from the {{site.data.keyword.Bluemix_notm}} region. {{site.data.keyword.IBM_notm}} provides all the resources that are needed to manage the locations.

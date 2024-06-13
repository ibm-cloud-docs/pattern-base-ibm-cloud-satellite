---

copyright:
  years: 2024
lastupdated: "2024-02-20"

subcollection: pattern-base-ibm-cloud-satellite

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Security design
{: #security-design}

From the security aspect, {{site.data.keyword.satellitelong_notm}} extends security access policies, logging, monitoring, and other controls to all {{site.data.keyword.satellitelong_notm}} locations. Customers can build new apps quickly, while maintaining strong regulatory controls. {{site.data.keyword.satelliteshort}} link works with customers existing security posture.

When {{site.data.keyword.satellitelong_notm}} location is configured in a cloud provider, the infrastructure credentials to the cloud provider must be provided to perform the necessary tasks. Customers looking to use their own security keys can Bring your own key (BYOK) or keep your own key (KYOK) on Red Hat Enterprise Linux (RHEL) and Red Hat CoreOS (RHCOS) hosts.

With {{site.data.keyword.satelliteshort}} link, all communication over it is encrypted by {{site.data.keyword.Bluemix_short}} and it uses a zero trust model. For more information, see [{{site.data.keyword.satelliteshort}} link](/docs/satellite?topic=satellite-link-location-cloud). IBM's direct link is also available as a connection option but with RHCOS hosts, [Connecting Locations with IBM Cloud using Direct Link](/docs/satellite?topic=satellite-direct-link-tutorial).

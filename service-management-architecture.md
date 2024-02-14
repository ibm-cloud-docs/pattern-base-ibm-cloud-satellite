---

copyright:
  years: 2024
lastupdated: "2024-1-8"

subcollection: base-ibm-cloud-satellite

keywords: Satellite, location

---

# Architecture decisions for service management
{: #service}

The following sections summarize the architecture decisions for service management for base {{site.data.keyword.satellitelong_notm}} pattern.

## Architecture decisions for monitoring
{: #monitoring}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | Satellite Location & Hosts | - IBM Satellite Monitoring Tool \n- IBM Cloud Monitoring |	IBM Satellite Monitoring Tool | By default, {{site.data.keyword.satellitelong_notm}} automatically monitors and resolves certain alerts for the Satellite location setup and host infrastructure that can be accessed through IBM Satellite Console and CLI. See [Default Monitoring for Satellite](https://cloud.ibm.com/docs/satellite?topic=satellite-monitor) for details. |
| | | | IBM Cloud Monitoring | IBM Satellite can be integrated with a customer-owned IBM Cloud Monitoring instance enabled for platform-level metrics to provide more detailed metrics. The Monitoring instance can be configured to collect metrics for both the Satellite location and Satellite-enabled services running in the Satellite location. |
| 2 | OpenShift Clusters | - Prometheus & Grafana on OpenShift \n- IBM Cloud Monitoring \n- Customer Monitoring Tool | IBM Cloud Monitoring | Manually deploy monitoring agents in OpenShift clusters to forward metrics to a customer-owned IBM Cloud Monitoring instance and get unified views of metrics for OpenShift clusters and other cloud services running at the Satellite location and within the Satellite managed-from region.  See [Setting up Monitoring for Clusters](https://cloud.ibm.com/docs/satellite?topic=satellite-monitor) for details. |
{: caption="Table 1. Architecture decisions for monitoring" caption-side="bottom"}

## Architecture decisions for logging
{: #logging}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | Satellite Location & Hosts | - IBM Satellite Log Analysis Tool \n- IBM Log Analysis | IBM Satellite Log Analysis Tool | By default, IBM Satellite automatically generates a set of logs for the Satellite location that can be accessed through the IBM Satellite built-in Log Analysis dashboard tools. See [Analyzing Logs for Satellite Location](https://cloud.ibm.com/docs/satellite?topic=satellite-health) for details. The Log Analysis instance can be configured to collect metrics for both the Satellite location and Satellite-enabled services running in the Satellite location. |
| | | | IBM Log Analysis | IBM Satellite can be integrated with a customer provisioned [IBM Log Analysis instance](https://cloud.ibm.com/docs/satellite?topic=satellite-health) enabled for platform-level logs to get a comprehensive view and tools to manage logs for IBM Satellite and other IBM Cloud resources. |
| 2 | OpenShift Clusters | = EFK stack on OpenShift \n- IBM Log Analysis \n- Customer Logging Tool | IBM Log Analysis | Manually deploy logging agents in OpenShift clusters to forward cluster logs to a custoimer-owned IBM Cloud Log Analysis instance and get a comprehensive view of logs for OpenShift clusters and other cloud services running at the Satellite location and within the Satellite managed-from region.  See [Setting up Logging for Clusters](https://cloud.ibm.com/docs/satellite?topic=satellite-health) for details. |
{: caption="Table 2. Architecture decisions for logging" caption-side="bottom"}

## Architecture decisions for auditing
{: #auditing}

| Architecture decision | Requirement | Option | Decision | Rationale |
|---|---|---|---|---|
| 1 | Satellite Location Events | IBM Cloud Activity Tracker | IBM Cloud Activity Tracker | Customer-owned IBM Cloud Activity Tracker instance for {{site.data.keyword.satellitelong_notm}} to forward audit events. IBM Cloud Activity Tracker tracks how users and applications interact with {{site.data.keyword.satellitelong_notm}}. It can be used to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. See [Auditing events for Satellite](https://cloud.ibm.com/docs/satellite?topic=satellite-at_events). |
| 2 | OpenShift Clusters | - IBM Cloud Activity Tracker \n- Customer tool | IBM Cloud Activity Tracker | Red Hat OpenShift on IBM Cloud automatically generates cluster management events and forwards these event logs to a customer-owned IBM Cloud Activity Tracker instance. See [Events for Satellite Clusters](https://cloud.ibm.com/docs/satellite?topic=satellite-at_events). |
{: caption="Table 3. Architecture decisions for auditing" caption-side="bottom"}

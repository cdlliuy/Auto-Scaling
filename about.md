---

 

copyright:

  years: 2015，2018
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}


# About
{: #about}

{{site.data.keyword.autoscaling}} is deprecated. As of 1 Augest 2019, you cannot provision new {{site.data.keyword.autoscaling}} instances on public region. Existing service instances are supported until 30 September 2019. To continue the auto-scaling capability for Cloud Foundray application in {{site.data.keyword.Bluemix_notm}}, please migrate to the NEW [built-in Auto-Scaling experience on Cloud Foundry application](https://{DomainName}/docs/cloud-foundry-public?topic=cloud-foundry-public-autoscale_cloud_foundry_apps). {:deprecated}

## Latest Version
1.6.4

## What is New: 
 * A series bug fix in Restful API and CLI plugin. 
 * Optimize the backend to ensure zero-down-time when Auto-Scaling service is in maintainance window. 
 * Optimize the metric pulling mechanism from Cloud Foundry.
 * For IBM Liberty Runtime,  add support for OpenJDK as well as IBM JDK. 
 * Add support for "throughput, response time, memory" based on dynamic scaling for IBM Swift Runtime. 


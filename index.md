---

 

copyright:

  years: 2015，2018
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}

# Getting started with {{site.data.keyword.autoscaling}} service
{: #get-started}


{{site.data.keyword.autoscaling}} is deprecated. As of 1 Augest 2019, you cannot provision new {{site.data.keyword.autoscaling}} instances on public region. Existing service instances are supported until 30 September 2019. To continue the auto-scaling capability for Cloud Foundray application in {{site.data.keyword.Bluemix_notm}}, please migrate to the NEW [built-in Auto-Scaling experience on Cloud Foundry application](https://{DomainName}/docs/cloud-foundry-public?topic=cloud-foundry-public-autoscale_cloud_foundry_apps). {: deprecated}

{{site.data.keyword.Bluemix_notm}} provides the capability to automatically optimize the resource allocation of your application through {{site.data.keyword.autoscaling}} service. You can use the {{site.data.keyword.autoscaling}} service to automatically increase or decrease the compute capacity of your Cloud Foundry applications. The number of application instances is adjusted dynamically based on the Auto-Scaling policy you defined.

{{site.data.keyword.autoscaling}} services  offers 2 scaling strategies: 
  * Dynamic Scaling:  
    With dynamic scaling, the runtime performance of your application will be monitored and evaluated through Auto-Scaling service in order to trigger a scaling in/out action dynamically. 
  * Scheduled Scaling:  
    If the workload of your application changes dramatically during the peak time and the spare time, you can create a schedule to full fill the different scaling requirements for different time periods. The application instance number could be customized in different range in different schedule, while the dynamic scaling rules still apply. 

{{site.data.keyword.autoscaling}} service offers 3 basic operations to end-user: 
  * Policy Configuration  
    {{site.data.keyword.autoscaling}} behaves based on the scaling policy definition. 
    Read section [{{site.data.keyword.autoscaling}} policy definition](./policy.html#policy_fields) to learn more about policy configuration. You can create and manage scaling policy through Console, RESTful API and Auto-Scaling CLI. 
  * Metric Statistics  
    {{site.data.keyword.autoscaling}} displays the most recent application runtime metrics through Console. 
  * Scaling History  
    For audit purpose, all actions triggered by {{site.data.keyword.autoscaling}} service are exposed as "Scaling History" which could be accessed through Console, RESTful API and CLI. 

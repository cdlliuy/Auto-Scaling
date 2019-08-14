---

 

copyright:

  years: 2015，2017
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}

# Manage {{site.data.keyword.autoscaling}} through Service Dashboard
{: #as-service}

{{site.data.keyword.autoscaling}} is deprecated. As of 1 Augest 2019, you cannot provision new {{site.data.keyword.autoscaling}} instances on public region. Existing service instances are supported until 30 September 2019. To continue the auto-scaling capability for Cloud Foundray application in {{site.data.keyword.Bluemix_notm}}, please migrate to the NEW [built-in Auto-Scaling experience on Cloud Foundry application](https://{DomainName}/docs/cloud-foundry-public?topic=cloud-foundry-public-autoscale_cloud_foundry_apps). 
{:deprecated}

To use the {{site.data.keyword.autoscaling}} service, complete the following steps:

1. On {{site.data.keyword.Bluemix_notm}} Dashboard, click *Create Resource*, and then select the {{site.data.keyword.autoscaling}} service from the `DevOps` section in the service catalog. Then there will be an overview of the {{site.data.keyword.autoscaling}} service.
2. Select the application that you want to bind the {{site.data.keyword.autoscaling}} service to and click *Create*.  Then the Restage Application window is displayed.<br/>
*Note:*  
You can bind **only ONE** {{site.data.keyword.autoscaling}} service to an application. If the application is already bound with another {{site.data.keyword.autoscaling}} service, do not select the application in this step. Otherwise, you will get the CWSCV2004E error.
3. In the Restage Application window, click *Restage* to restage your application before you use the new {{site.data.keyword.autoscaling}} service that you just added. <br/><ul><li> For Liberty application, the {{site.data.keyword.autoscaling}} is auto-configured and ready for use after the application restage.</li> <li>For Node.js or Swift applications, you must update your application code to enable the {{site.data.keyword.autoscaling}} service before pushing the application to {{site.data.keyword.Bluemix_notm}}. Refer to <ul><li> [Configuring Node.js apps with the {{site.data.keyword.autoscaling}} service](./guide.html#node-asagent) </li> <li>[Configuring Swift apps with the {{site.data.keyword.autoscaling}} service](./guide.html#swift-asagent)</li></ul></ul> 

Now you can configure the {{site.data.keyword.autoscaling}} policy, see the metric statistics, or view the scaling history for the application.
<dl>
<dt>Policy Configuration</dt>
<dd>
Create, update and view your scaling policy of your application.  Details for policy definition can be found in [Define Dynamic Scaling rule through Console](../policy.html)  
and [Metric type supported for different runtimes](../metric.html)  

<dt>Metric Statistics</dt>
<dd>Displays the metric statistics for the instances of your application. You can see the average statistics and select specific instance to see its statistic.  
  
*Note:* The time shown in the X-axis of the line chart record is based on the default timezone setting of your browser</dd>

<dt>Scaling History</dt>
<dd>Displays the scaling history of your applications.<ul>
<li> Past Week: Displays the scaling history for the past week.
<li> Past Month: Displays the scaling history for the past month.
<li> Custom Range: You can set the time period.</ul>

*Note:* You can filter the history record by selecting Scaling Status or Scaling In/Out. The **StartTime** of scaling history record is based on the **Time Zone** setting in this tab</dd>
</dl>


---

 

copyright:

  years: 2015，2018
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}


# Metric type supported for different runtimes
{: #metric_types}

{{site.data.keyword.autoscaling}} is deprecated. As of 1 Augest 2019, you cannot provision new {{site.data.keyword.autoscaling}} instances on public region. Existing service instances are supported until 30 September 2019. To continue the auto-scaling capability for Cloud Foundray application in {{site.data.keyword.Bluemix_notm}}, please migrate to the NEW [built-in Auto-Scaling experience on Cloud Foundry application](https://{DomainName}/docs/cloud-foundry-public?topic=cloud-foundry-public-autoscale_cloud_foundry_apps). 
{:deprecated}

| Metric name | Data Source | Supported Runtime type |
|-------------|----------------------| ------------------- |
| *Heap* |   Auto-Scaling data collector    | Liberty for Java, Node.js SDK |
| *Throughput* |  Auto-Scaling data collector| Liberty for Java, Node.js SDK, Swift (with Kitura) |
| *Response time* |   Auto-Scaling data collector | Liberty for Java, Swift (with Kitura)|
| *Memory*   | Auto-Scaling data collector   |  Liberty for Java, Node.js SDK, Swift (with Kitura)  |
| *Memory*   | Cloud Foundry   |  Runtimes other than Liberty for Java, Node.js SDK and Swift (with Kitura) |

{: caption="Table 2. Supported metric types" caption-side="top"}

*Note:* You can define multiple dynamic scaling rules for more than one metric type. However, the {{site.data.keyword.autoscaling}} service does not detect conflicts between scaling policies. When you define the scaling policy, you must ensure that multiple scaling rules do not conflict with one another. Otherwise, you might see the total instance number fluctuates because the application scales in 1 minute and scales out the next.  



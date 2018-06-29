---

 

copyright:

  years: 2015，2017
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# FAQ
{: #FAQ}

**Q:  Why I only see "Memory" metric type available on my tomcat application?**  
A: {{site.data.keyword.autoscaling}} service uses [Liberty Monitor-1.0 feature -> ServletStats MXBean](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_mon_webapp.html) to evaluate the web application's workload. Your application must be deployed as Liberty web application so that Liberty web container and Liberty Monitor-1.0 feature are loaded to collect the metrics.  
Using tomcat server does not use Liberty web container, therefore no metrics data is collected.

**Q:  Why the response time and throughput metrics are both 0 for my spring-boot application?**  
A: {{site.data.keyword.autoscaling}} service uses [Liberty Monitor-1.0 feature -> ServletStats MXBean](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_mon_webapp.html) to evaluate the web application's workload.  
When you run a Spring Boot application as a "Main-Class" app, the Liberty buildpack only provides java environment for you, and the app actually runs in the Spring embedded Tomcat container, thus no metrics data will be collected by the {{site.data.keyword.autoscaling}} service.   
In this case, you must run your app as a Liberty WAR in order to work with {{site.data.keyword.autoscaling}} service. You can refer this doc [http://www.adeveloperdiary.com/java/spring-boot/deploy-spring-boot-application-ibm-liberty-8-5/](http://www.adeveloperdiary.com/java/spring-boot/deploy-spring-boot-application-ibm-liberty-8-5/) to deploy Spring Boot application in IBM Liberty.

**Q: Why the response time/throughput reported by load test tools (i.e. ab) is different from the metrics reported by {{site.data.keyword.autoscaling}} dashboard?**  
A: {{site.data.keyword.autoscaling}} counts every request and calculates the mean throughput and response time every 2 minutes, but "ab" calculate the mean throughput just upon the real test time.  
For example,  a user tried to issue 10000 requests to an application with "ab" by using "ab -c 50 -n 10000 {APP_URL}" .  Once the test was done, "ab" showed the test lasted 93.66 seconds, and the mean throughput was 106.76/sec.  
On the other hand,  what can be found in {{site.data.keyword.autoscaling}} dashboard is 2 points of throughput metric, one was 34 req/sec and another was 50 req/sec. The default collection interval of {{site.data.keyword.autoscaling}} service is 120 seconds. 
   * During the first 120 seconds, there were 4000 requests, so the mean throughput provided by {{site.data.keyword.autoscaling}} was 4000/120=33.3 (≈34) req/sec
   * During then second 120 seconds, there were 6000 requests, so the mean throughput provided by {{site.data.keyword.autoscaling}} was 6000/120=50 req/sec 

It is obviously that the different throughput value was caused by the insufficient test duration, which is just about 1.5 minutes. So the "ab" test could be longer, the throughput metric from both side will be similar. 
So, please launch the load test with a bit longer test period if you have interests to check response time and throughput metrics. 

**Q:  Why the response time and throughput metrics are both 0 when running load test against IBM liberty sample app?**  
A: The requests going to static HTML page are not captured since no servlet is hit.  
For example, if you deploy the starter web-application of Liberty for Java Runtime and run load test against its root URL which is a static HTML page, the reported Response time/Throughput metric will be 0.  To workaround this,  you need to run load test again its servlet (i.e. https://\<sample_app_url\>/SimpleServlet)

**Q:  Why I got connection error for {{site.data.keyword.autoscaling}} service in my application log?**  
A:  For Liberty for Java, Node.js and Swift,  {{site.data.keyword.autoscaling}} service replies on an embedded "{{site.data.keyword.autoscaling}} agent" which is injected into the application container to report its runtime metrics with "push" model.
The agent sents the metric back to {{site.data.keyword.autoscaling}} service through https. When the Cloud Foundry or the whole platform is unstable or suffering from connection issues,  the "push" model doesn't work, then "{{site.data.keyword.autoscaling}}" agent  writes down the connection errors honestly into the application log.  
In most of the case, the connection will recover automatically once the platform become stable. Otherwise, please contact us with IBM support. 

**Q:  Why the throughput metrics is 0 when running load test against Node.js app?**  
A: {{site.data.keyword.autoscaling}} service uses a Node.js module "bluemix-autoscaling-agent" to collect the application metrics. <br/>
Many Node.js application developers uses "express", "http" and etc libraries to host web server.  In order to capture the throughput metric from these web server implementation, "bluemix-autoscaling-agent" must be imported as early as possible, i.e. prior to  "express" or "http". Otherwise, it will fail to report throughput metric.  
```
  var agent = require('bluemix-autoscaling-agent');
  var http = require('http');
  var server = http.createServer(function handler(req, res) {
    console.log("Hello!");
    
    }).listen(process.env.PORT || 3000);
  console.log('App is listening on port 3000');
```

**Q:  What is the authorization token required to access {{site.data.keyword.autoscaling}} API?**  
A:  {{site.data.keyword.autoscaling}} grants user authority according to the user role control of Cloud Foundry.   Only space developer would access {{site.data.keyword.autoscaling}} resources through dashabord, API or CLI.  <br/>
To access {{site.data.keyword.autoscaling}} API,  you need to provide a valid user token of Cloud Foundry.  You can get your user token through command [`cf oauth-token`](http://cli.cloudfoundry.org/en-US/cf/oauth-token.html).  <br/>
The output of the command is formatted as "bearer <bear token>".  Please keep this output and add it as the authorization header in your  API request. 
```
  curl -H "Authorization: bearer <bear token>" -X GET {REQUEST_URL} 
```


**Q:  What will happen when changing instances number by % ?**  
A:  When changing instances number by % as defined in [`policy`](../policy.html#policy_fields), the target application would scale out/in more quickly with a large instance basis, i.e. when you have 100 running instances. <br/>
{{site.data.keyword.autoscaling}} will make sure the target instance number is an integer value with [`ceil function`](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions) when changing by % is defined.
For example
   * Big number of the running instances, let's say 90, scale out  % is set to 15%  ==> 14 instances should be scaled out
   * Small number of the running instances, let's say 4, scale out % is set to 10%  ==> 1 instance should be scaled out

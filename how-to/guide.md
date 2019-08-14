---

 

copyright:

  years: 2015，2017
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}

# Developer guide to configure your applications with {{site.data.keyword.autoscaling}} service
{: #guide}

{{site.data.keyword.autoscaling}} is deprecated. As of 1 Augest 2019, you cannot provision new {{site.data.keyword.autoscaling}} instances on public region. Existing service instances are supported until 30 September 2019. To continue the auto-scaling capability for Cloud Foundray application in {{site.data.keyword.Bluemix_notm}}, please migrate to the NEW [built-in Auto-Scaling experience on Cloud Foundry application](https://{DomainName}/docs/cloud-foundry-public?topic=cloud-foundry-public-autoscale_cloud_foundry_apps). 
{:deprecated}

## Configuring Node.js apps with the {{site.data.keyword.autoscaling}} service
{: #node-asagent}

To enable the {{site.data.keyword.autoscaling}} service with your Node.js apps, besides service provision and binding steps, you need to complete the following steps as well before pushing the app to {{site.data.keyword.Bluemix_notm}}.
1. Enable `blumix-autoscaling-agent` in your application.
  + Update the package.json to create a dependency entry for *blumix-autoscaling-agent*. For example:<br/> 
  `"dependencies": {
    "bluemix-autoscaling-agent": "*"
  }`
2. Start *blumix-autoscaling-agent* with your application.
There are two ways to start the agent. Either works. 
  + The first way is to update the package.json file by adding `"start": "node -r bluemix-autoscaling-agent  app.js"` to *script* section. For example:<br/>
```
  "scripts": {
    "start": "node -r bluemix-autoscaling-agent app.js"
  } 
```
  + The second way is to update your main file to add the agent declaration `var as_agent = require('bluemix-autoscaling-agent');` <br/>
  *Note:* `bluemix-autoscaling-agent` must be initialized prior to the npm modules you want to monitor, so you must call `require('bluemix-autoscaling-agent');` **prior to** the other `require` statements. <br/>
  The following code snippet shows a complete entry js file with the auto-scaling agent declaration.<br/> 
```
  var agent = require('bluemix-autoscaling-agent');
  var http = require('http');
  var server = http.createServer(function handler(req, res) {
    console.log("Hello!");
    
    }).listen(process.env.PORT || 3000);
  console.log('App is listening on port 3000');
```

3. (Optional) Set heap limit if you want to trigger scaling based on heap usage. Update package.json to define `max-old-space-size` based on the memory that you allocate for your app. <br/> For example: 
  `
  "scripts": {
    "start": "node --max-old-space-size=600 app.js"
  }
  `
<br/> If the value is not set when you start your application, the default Node.js heap limit 1.4GB is used as the maximum heap size regardless how much memory your app is allocated, which might lead to improper auto-scaling decisions.<br/>

A full sample of package.json:
```
  {
    "name": "Your-App",
    "version": "0.0.1",
    "description": "A sample nodejs app for Bluemix",
    "scripts": {
      "start": "node -r bluemix-autoscaling-agent --max-old-space-size=600 app.js"
    },
    "dependencies": {
      "bluemix-autoscaling-agent": "*"
    },
    "repository": {},
    "engines": {
      "node": "0.12.x"
    } 
  }
``` 

## Configuring Swift apps with the {{site.data.keyword.autoscaling}} service
{: #swift-asagent}

To enable the {{site.data.keyword.autoscaling}} service with your Swift apps, besides service provision and binding steps, you need to complete the following steps as well before pushing the app to {{site.data.keyword.Bluemix_notm}}.

1. Update Package.swift file to add dependency declaration of package SwiftMetrics: 
```
   Package(url: "https://github.com/RuntimeTools/SwiftMetrics.git" , majorVersion: 1)
```
   A sample Package.swift file is as below: 
```
import PackageDescription
let package = Package(
    name: "Your-App",
    targets: [
      Target(name: "Your-App", dependencies: [])
    ],
    dependencies: [
      .Package(url: "https://github.com/RuntimeTools/SwiftMetrics.git" , majorVersion: 1)
    ]) 
```
2. Add the SwiftMetrics modules to your Swift app. 
 + Import the packages.
```   
import SwiftMetrics
import SwiftMetricsKitura
import SwiftMetricsBluemix
```
 + Initialize and start the agent: 
    +  Declare variables of the SwiftMetrics and SwiftMonitor in your app.
```   
      let sm: SwiftMetrics
      let monitor: SwiftMonitor
```
    +  Create instances of the SwiftMetrics and SwiftMonitor classes in your app.
```   
      sm = try SwiftMetrics()
      _ = SwiftMetricsKitura(swiftMetricsInstance: sm)
      _ = SwiftMetricsBluemix(swiftMetricsInstance: sm)
      monitor = sm.monitor()
```
    +  A sample of a Swift app that uses SwiftMetrics and Kitura is shown below:
```
      import Configuration
      import CloudFoundryConfig
      import Kitura
      import SwiftyJSON
      import Dispatch
      import LoggerAPI
      import CloudFoundryEnv
      import SwiftMetrics
      import SwiftMetricsKitura
      import SwiftMetricsBluemix
      import Foundation
      public class Controller {
          let router: Router
          let configMgr: ConfigurationManager
          var jsonEndpointEnabled: Bool = true
          var jsonEndpointDelay: UInt32 = 0

          // Declare variables of the SwiftMetrics and SwiftMonitor in your app
          let sm: SwiftMetrics
          let monitor: SwiftMonitor

          var port: Int {
              get { return configMgr.port }
          }
          var url: String {
              get { return configMgr.url }
          }
          init() throws {
              configMgr = ConfigurationManager().load(.environmentVariables)

              //Create instances of the SwiftMetrics and SwiftMonitor classes in your app
              sm = try SwiftMetrics()
              _ = SwiftMetricsKitura(swiftMetricsInstance: sm)
              _ = SwiftMetricsBluemix(swiftMetricsInstance: sm)
              monitor = sm.monitor()

              // All web apps need a Router instance to define routes
              router = Router()
              // Serve static content from "public"
              router.all("/", middleware: StaticFileServer())
              // Basic GET request
              router.get("/hello", handler: getHello)
          }
          
          public func getHello(request: RouterRequest, response: RouterResponse, next: @escaping () -> Void) throws {
              Log.debug("GET - /hello route handler...")
              response.headers["Content-Type"] = "text/plain; charset=utf-8"
              try response.status(.OK).send("Hello from Kitura-Starter!").end()
          }

      }
```

*Note:* Currently only Swift application using Kitura framework is supported to use {{site.data.keyword.autoscaling}} service.

## Reference
* [{{site.data.keyword.autoscaling}} agent for Node.js](https://www.npmjs.com/package/bluemix-autoscaling-agent){:new_window}
* [{{site.data.keyword.autoscaling}} agent for Swift](https://github.com/RuntimeTools/SwiftMetrics){:new_window}

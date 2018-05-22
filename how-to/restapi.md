---

 

copyright:

  years: 2015，2017
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Manage {{site.data.keyword.autoscaling}} service through RESTful API
{: #RESTAPI}

[{{site.data.keyword.autoscaling}} RESTful API](https://new-console.{DomainName}/apidocs/48) provides an alternative way to manage {{site.data.keyword.autoscaling}} service. It exposes HTTP RESTful API to manage scaling policy and view scaling history. 

{{site.data.keyword.autoscaling}}  service RESTful API document explains the details API definition: [Rest API of IBM {{site.data.keyword.autoscaling}} for {{site.data.keyword.Bluemix_notm}}](https://new-console.{DomainName}/apidocs/48).

Before using the [{{site.data.keyword.autoscaling}} RESTful API](https://new-console.{DomainName}/apidocs/48) to manage the {{site.data.keyword.autoscaling}} service, please complete below prerequisites with this guide:
* Bind the {{site.data.keyword.autoscaling}} service with your application as described in previous section.
* Acquire the `AccessToken`
<br/>The user must login CloudFoundry and provide a valid access token. The `AccessToken` can be obtained by command [``cf oauth-token``](https://cli.cloudfoundry.org/en-US/cf/oauth-token.html).
 *Note:*  The `AccessToken` may be expired after a period. If you get a 401 Unauthorized response for RESTful API request again, you need to repeat above steps to refresh `AccessToken`. 
  
* Get endpoint of {{site.data.keyword.autoscaling}} API server <br/>
You can obtain the endpoint of {{site.data.keyword.autoscaling}} API server by checking the environment variable of `VCAP_SERVICE`  -> {{site.data.keyword.autoscaling}} service  ->  `api_url` 
```
   > cf env "Your-App"
   Getting env variables for app Your-App in org Your-Org / space Your-Space as ...
   OK

   System-Provided:
   {
     "VCAP_SERVICES": {
     "Auto-Scaling": [
     {
      "credentials": {
       "agentPassword": "....",
       "agentUsername": "agent",
       "api_url": "https://ScalingAPI.ng.bluemix.net",
       "app_id": "aa8d19b6-eceb-4b6e-b034-926a87e98a51",
       "service_id": "8f482f78-58d8-493c-829b-635e4cbfd817",
       "url": "https://Scaling.ng.bluemix.net"
      },
    ...
     }
   ...
```  

* Get the `app_id` of the application that you want to scale in or out with 
 the `cf app APPNAME --guid` command:
```
   > cf app Your-App --guid
   aa8d19b6-eceb-4b6e-b034-926a87e98a51
```
* Determine the proper API URL for your request: Please check [{{site.data.keyword.autoscaling}} RESTful API](https://new-console.{DomainName}/apidocs/48) for endpoint definition of {{site.data.keyword.autoscaling}} RESTful API

A sample script to invoke {{site.data.keyword.autoscaling}} service RESTful API with *curl* and *jq*:
```
cf login -a http://api.ng.bluemix.net -u Your-UserName -p Your-Password --skip-ssl-validation

accessToken=$(cf oauth-token)
appId=$(cf app Your-App --guid)
API_url=$(cf curl /v2/apps/${appId} --guid`/env | jq '.system_env_json.VCAP_SERVICES."Auto-Scaling"[0].credentials.api_url' -r)
policyJson="./policy.json"

echo "Create scaling policy using API :"
createPolicyUrl="${API_url}/v1/autoscaler/apps/${appId}/policy"
    
curl $createPolicyUrl -X 'PUT' -H 'Content-Type:application/json' -H 'Accept:application/json'  -H "Authorization:$accessToken" --data-binary @${policyJson} 
```

---

 

copyright:

  years: 2015，2017
lastupdated: "2017-02-22"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:generic: data-hd-programlang="generic"}
{:java: data-hd-programlang="java"}
{:ruby: data-hd-programlang="ruby"}
{:c#: data-hd-programlang="c#"}
{:objectc data-hd-programlang="objectc"}
{:python: data-hd-programlang="python"}
{:javascript: data-hd-programlang="javascript"}
{:php: data-hd-programlang="php"}
{:swift: data-hd-programlang="swift"}

{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:ruby: #ruby .ph data-hd-programlang='ruby'}
{:shortdescription: .shortdesc}

# Getting started with Redis (Experimental)
{: #redis}

This community service is as-is and is provided for development and experimentation purposes only. 

Redis is an open source, key-value store offering that supports memory and persistent data store, and it provides applications with powerful data structures, such as strings, hashes, lists, sets, and sorted sets. 
{:shortdesc} 

## Using the Redis service

You can use the `Node.js`{: javascript} `Java`{: java} `Ruby`{: ruby} sample application to try the Redis service.

To use the Redis service, complete the following steps:
  1. Manage the Redis service instance by using the command line interface. 
     First, create a Redis service instance by using the `cf create-service` command.
	 ```
	 cf create-service redis 100 redis01
     ```
	 
	 The `cf create-service` command creates a Redis service instance with a name of *redis01* in your space. You can type the `cf services` command to list all available service instances that are created. 
	 ```
	 cf services
	 ```
	 Next, bind the service to your application by using the `cf bind-service` command:
	 ```
	 cf bind-service AppName redis01
	 ```
  2. Connect to the Redis service instance in your application. 
     After you bind a Redis service instance to the application, a configuration that is similar to the following example is added to your VCAP_SERVICES environment variable. You must use the configuration in your VCAP_SERVICES environment variable to connect to the Redis service instance.
	 ```
	 {
        "redis": [
           {
              "name": "redis-c40d4",
              "credentials": {
                 "hostname": "10.0.116.27",
                 "host": "10.0.116.27",
                 "port": 5556,
                 "password": "c4c63e2e-1988-473d-b88c-20ea25f5605c",
                 "name": "0d7d72f2-2bfe-4d5e-b66e-3249d2090cd6"
              }
            }
         ]
	      }
     ```
	 {: javascript}
	 
	 
     To obtain the service credentials in the VCAP_SERVICES environment variable and that are required to connect to the Redis service instance, you can use the following code snippet:
     ```
     if (process.env.VCAP_SERVICES) {
         var env = JSON.parse(process.env.VCAP_SERVICES);
         var credentials = env['redis'][0].credentials;
     } else {
         var credentials = {
             "host": "127.0.0.1",
             "port": 5556,
             "username" : "user1",
             "password" : "secret"
         }
     };
     ```
     {: javascript}


     ```
     if (process.env.VCAP_SERVICES) {
         var env = JSON.parse(process.env.VCAP_SERVICES);
         var credentials = env['redis'][0].credentials;
     } else {
         var credentials = {
             "host": "127.0.0.1",
             "port": 5556,
             "username" : "user1",
             "password" : "secret"
         }
     };
     ```


     ```
     public Object[] getServiceInfo() throws Exception{
         CloudEnvironment environment = new CloudEnvironment();
         if ( environment.getServiceDataByLabels("redis").size() == 0 ) {
             throw new Exception( "No Redis service is bund to this app!!" );
         }

         Object[] info = new Object[3];
         Map credential = (Map)((Map)environment.getServiceDataByLabels("redis").get(0)).get( "credentials" );    

         info[0] = (String)credential.get( "host" );
         info[1] = (Integer)credential.get( "port" );
         info[2] = (String)credential.get( "password" );

         return info;
       }
     ```
     {: java}


     ```
     before do
       @length = @@redis.LLEN("notes")
       @time = Time.now.to_s
     end

     configure do

       @env = {}

       ENV.each do |key, value|
         begin
           hash = JSON.parse(value)
           @env[key] = hash
         rescue
           @env[key] = value
         end
       end

       unless @env.nil?
         @appInfo = @env["VCAP_APPLICATION"]
         LOGGER.debug("appInfo: #{ @appInfo }")
         @services = @env["VCAP_SERVICES"]
         LOGGER.debug("services: #{ @services }")
       end

       unless @services.empty?
         begin
           # Assuming that this is bluemix
           redis_key = services.keys.select { |svc| svc =~ /redis/i }.first
           redis = services[redis_key].first['credentials']
           redis_conf = {:host => redis['hostname'], :port => redis['port'], :password => redis['password']}
           @@redis = Redis.new redis_conf
         end
       end

     end
     ```
     {: ruby}



# rellinks
{: #rellinks}

## samples
{: #samples}

* [Tutorial: Make your application elastic on {{site.data.keyword.Bluemix_notm}}](http://www.ibm.com/developerworks/cloud/library/cl-autoscale-app/index.html){:new_window}

## sdk
{: #sdk}

* [Rest API of IBM {{site.data.keyword.autoscaling}} for {{site.data.keyword.Bluemix_notm}}](https://new-console.{DomainName}/apidocs/48){:new_window}

## general
{: #general}

* [Scaling containers](https://www.{DomainName}/docs/containers/container_creating_ov.html#container_group_ov){:new_window}
* 

---

 

copyright:

  years: 2015，2017
lastupdated: "2017-08-09"  
 

---

{:codeblock: .codeblock}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Sample policy file for {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.autoscaling}} service

## Simple policy definition for Dynamic scaling only
```json
{
    "instanceMinCount": 1,
    "instanceMaxCount": 5,
    "policyTriggers": [
        {
            "metricType": "JVMHeapUsage",
            "statWindow": 300,
            "breachDuration": 600,
            "lowerThreshold": 30,
            "upperThreshold": 80,
            "instanceStepCountDown": 1,
            "instanceStepCountUp": 2,
            "stepDownCoolDownSecs": 600,
            "stepUpCoolDownSecs": 600
        }
    ]
}
```

## A complete policy definition for both Dynamic scaling and Scheduled Scaling
```json
{
    "instanceMinCount": 1,
    "instanceMaxCount": 5,
    "policyTriggers": [
      {
            "metricType": "JVMHeapUsage",
            "statWindow": 130,
            "breachDuration": 600,
            "lowerThreshold": 30,
            "upperThreshold": 80,
            "instanceStepCountDown": 100,
            "instanceStepCountUp": 4,
            "stepDownCoolDownSecs": 600,
            "stepUpCoolDownSecs": 600,
            "scaleInAdjustment": "changePercentage"
        },
        {
            "metricType": "Throughput",
            "statWindow": 300,
            "breachDuration": 600,
            "lowerThreshold": 30,
            "upperThreshold": 80,
            "instanceStepCountDown": 100,
            "instanceStepCountUp": 200,
            "stepDownCoolDownSecs": 600,
            "stepUpCoolDownSecs": 600,
            "scaleInAdjustment": "changePercentage",
            "scaleOutAdjustment": "changePercentage"
        },
       {
            "metricType": "Memory",
            "statWindow": 140,
            "breachDuration": 140,
            "lowerThreshold": 30,
            "upperThreshold": 80,
            "instanceStepCountDown": 100,
            "instanceStepCountUp": 200,
            "stepDownCoolDownSecs": 140,
            "stepUpCoolDownSecs": 140,
            "scaleInAdjustment": "changePercentage",
            "scaleOutAdjustment": "changePercentage"
        }
    ],
    "schedules" : {
        "timezone": "(GMT +08:00) Asia/Taipei",
        "recurringSchedule": [
            {
                "startTime": "00:00",
                "endTime": "23:59",
                "repeatOn": "[\"1\",\"2\",\"3\",\"4\",\"5\",\"6\",\"7\"]",
                "minInstCount": "1",
                "maxInstCount": "5"
            }
        ],
        "specificDate": [
            {
                "startDate": "2015-06-19",
                "startTime": "00:00",
                "endDate": "2015-06-19",
                "endTime": "23:59",
                "minInstCount": "1"
            },
            {
                "startDate": "2015-06-28",
                "startTime": "00:00",
                "endDate": "2015-06-28",
                "endTime": "23:59",
                "minInstCount": "1"
            }
        ]
    }
}
```
*Note:* You can define multiple dynamic scaling rules for more than one metric type. However, the {{site.data.keyword.autoscaling}} service does not detect conflicts between scaling policies. When you define the scaling policy, you must ensure that multiple scaling rules do not conflict with one another. Otherwise, you might see the total instance number fluctuates because the application scales in 1 minute and scales out the next. 

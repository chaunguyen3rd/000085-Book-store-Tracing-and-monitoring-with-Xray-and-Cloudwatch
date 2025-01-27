---
title : "Create custom metric"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2. </b> "
---
CloudWatch metric provides several metrics for Lambda functions such as the number of times the function is executed, the execution time of each time, error rates, and throttle count. To see the metrics of a certain function we do the following:
1. Open the Lambda function's dashboard
2. Click the function **book-list**
3. Click the **Monitor** tab, select **Metrics**
- Metrics are displayed
4. Next we will create a new metric that sums up the number of hits to DynamoDB that fail
5. Click the **Code** tab, add the following line of code at the top of the function
```
client_cloudwatch = boto3.client('cloudwatch')
```
- Add the following code to the except block of scan DynamoDB table before returning results


```
        client_cloudwatch.put_metric_data(
            Namespace='BooksList_Lambda',
            MetricData=[
                {
                    'MetricName': 'FailedConnectToDynamoDB',
                    'Dimensions': [
                        {
                            'Name': 'env',
                            'Value': 'staging'
                        },
                    ],
                    'Value': 1.0,
                    'Unit': 'Seconds'
                },
            ]
        )
```  
- The code helps you to create a new metric and push the data into it every time the connection to DynamoDB fails

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-1.png?featherlight=false&width=90pc)

6. Give the function permission to access and push data into metric
- Click **Configure** tab, then select **Permissions** in the left menu. Click to lambda function's role

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-2.png?featherlight=false&width=90pc)

- Expand the **BooklistRole..** policy, then click **Edit**

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-3.png?featherlight=false&width=90pc)

- Click **JSON** tab, add below script to editor:

```
{
    "Sid": "VisualEditor0",
    "Effect": "Allow",
    "Action": "cloudwatch:PutMetricData",
    "Resourse": "*"
},
```

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-4.png?featherlight=false&width=90pc)

- Click **Review policy**, then click **Save change**

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-5.png?featherlight=false&width=90pc)

7. Call GET API again
8. Open the CloudWatch dashboard
9. Click on Metrics in the left menu, then click **All metrics**
10. In the **Custom namespaces** section appears the metric you created - **BooksList_Lambda**. Click it

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-6.png?featherlight=false&width=90pc)

11. Press **env**

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-7.png?featherlight=false&width=90pc)

12. Select **staging**, data display chart

![ChangeCode](/images/2-cloudwatch-monitor/2-1-custom-metric-8.png?featherlight=false&width=90pc)

13. You can choose to display parameters according to 1 day or 1 week time, line or number displayed at the top of the graph
So we have created a custom metric. Next step we will use it to create a CloudWatch Alarm
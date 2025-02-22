---
title : "Create custom metric"
date : 2025-02-11
weight : 2
chapter : false
pre : " <b> 2.2. </b> "
---

CloudWatch metric provides several metrics for Lambda functions such as the number of times the function is executed, the execution time of each time, error rates, and throttle count. To see the metrics of a certain function we do the following steps.

1. Open [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Click **Functions** on the left menu.
    - Choose the **books_list** function.
      ![CloudWatchMetrics](/images/temp/1/5.png?width=90pc)

2. At **books_list** page.
    - Click the **Monitor** tab.
    - You can see the **CloudWatch metrics** are displayed.
      ![CloudWatchMetrics](/images/temp/1/12.png?width=90pc)

3. Next, we will create a new custom metric that sums up the number of hits to DynamoDB that fail. At **books_list** page.
    - Click the **Code** tab.
    - Copy the following code.

      ```python
      import boto3
      import os
      import simplejson as json

      TABLE = os.environ['TABLE_NAME']

      # Get the service resource
      dynamodb = boto3.resource('dynamodb')
      table = dynamodb.Table(TABLE)
      cloudwatch = boto3.client('cloudwatch')

      header_res = {
          "Content-Type": "application/json",
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Methods": "OPTIONS,POST,GET,DELETE",
          "Access-Control-Allow-Headers": "Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token",
      }

      secondary_index = "name-index"


      def lambda_handler(event, context):
          try:
              books_data = table.scan(
                  TableName='Book',
                  IndexName=secondary_index
              )

              books = books_data.get('Items', [])

              for book in books:
                  data_comment = table.query(
                      TableName=TABLE,
                      KeyConditionExpression="id = :id AND rv_id > :rv_id",
                      ExpressionAttributeValues={
                          ":id": book['id'],
                          ":rv_id": 0
                      }
                  )

                  book['comments'] = data_comment['Items']

              return {
                  "statusCode": 200,
                  "headers": header_res,
                  "body": json.dumps(books, use_decimal=True)
              }
          except Exception as e:
              print(f'Error getting items: {e}')

              cloudwatch.put_metric_data(
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

              raise Exception(f'Error getting items: {e}')
      ```

    - Click the **Deploy** button.
      ![CloudWatchMetrics](/images/temp/1/13.png?width=90pc)

4. At **books_list** page.
    - Click the **Configuration** tab.
    - Click the **Permissions** on the left menu.
    - Click the **fcj-book-store-BooksListRole-...** at **Role name**.
      ![CloudWatchMetrics](/images/temp/1/14.png?width=90pc)

5. At **fcj-book-store-BooksListRole-...** page.
    - Click the **Permissions** tab.
    - Click the **+** at **BooksListRolePolicy0** Policy name.
    - Click the **Edit** button.
      ![CloudWatchMetrics](/images/temp/1/15.png?width=90pc)

6. At **Step 1: Modify permissions in BooksListRolePolicy0** page.
    - Click the **JSON** tab.
    - Copy the following code to the **Policy editor**.

      ```json
      {
          "Sid": "VisualEditor0",
          "Effect": "Allow",
          "Action": "cloudwatch:PutMetricData",
          "Resource": "*"
      },
      ```

      ![CloudWatchMetrics](/images/temp/1/16.png?width=90pc)
    - Scroll down to the bottom and click the **Next** button.
      ![CloudWatchMetrics](/images/temp/1/17.png?width=90pc)

7. At **Step 2: Review and save** page.
    - Click the **Save changes** button.
      ![CloudWatchMetrics](/images/temp/1/18.png?width=90pc)

8. Open **Postman** to recall the api, the error returned is **Internal server error**.
    ![CloudWatchMetrics](/images/temp/1/9.png?width=90pc)

9. Back to **books_list** Lambda function page.
    - Click the **Monitor** tab.
    - Click the **View CloudWatch logs** button.
      ![CloudWatchLog](/images/temp/1/6.png?width=90pc)

10. At **CloudWatch** page.
    - Click the **All metrics** on the left menu.
    - Click the **BooksList_Lambda** at the **Custom namespaces**.
      ![CloudWatchMetrics](/images/temp/1/19.png?width=90pc)
    - Next, click the **env**.
      ![CloudWatchMetrics](/images/temp/1/20.png?width=90pc)
    - Click the **staging**.
    - Click the **Add to graph**.
      ![CloudWatchMetrics](/images/temp/1/21.png?width=90pc)
    - After the graph is refreshed, hover the **Blue** point on the graph.
    - Then, you can see the information.
      ![CloudWatchMetrics](/images/temp/1/22.png?width=90pc)

So we created a custom metric successfully. Next step we will use it to create a CloudWatch Alarm.

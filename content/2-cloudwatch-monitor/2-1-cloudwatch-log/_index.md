---
title : "Debugging with CloudWatch logs"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1. </b> "
---
1. Open **Postnam** to call api
- Add a new tab
- Select method **GET**
- Paste the API URL recorded in the previous step and add `books` at the end
- Press **Send**

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-1.png?featherlight=false&width=90pc)

- After completing the data of the **Books** table is returned

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-2.png?featherlight=false&width=90pc)

2. Open console of [AWS Lambda]()

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-3.png?featherlight=false&width=90pc)

3. Click functions **books_list**

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-4.png?featherlight=false&width=90pc)

4. Select the **Monitor** tab
- Click **View logs CloudWatch**

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-5.png?featherlight=false&width=90pc)

5. You will see all the logs saved each time the function **books_list** is executed

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-6.png?featherlight=false&width=90pc)

6. Click on the latest log

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-6.png?featherlight=false&width=90pc)

You should see the function running normally and without any errors. Next, we will fix the code to make the function run error.

7. Modify the code as follows:


```
import json
import boto3
from decimal import *
from boto3.dynamodb.types import TypeDeserializer


client = boto3.client('dynamodb') 
serializer = TypeDeserializer()
client_cloudwatch = boto3.client('cloudwatch')

class DecimalEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Decimal):
            return str(obj)
        return json.JSONEncoder.default(self, obj)
            
def deserialize(data):
    if isinstance(data, list):
        return [deserialize(v) for v in data]

    if isinstance(data, dict):
        try:
            return serializer.deserialize(data)
        except TypeError:
            return {k: deserialize(v) for k, v in data.items()}
    else:
        return data

def lambda_handler(event, context):
    try:
        data_books = client.scan(
            TableName='Book',
            IndexName='name-index'
        )
    except Exception as e:
        print(e)
    
    format_data_books = deserialize(data_books["Items"])
    for book in format_data_books:
        try:
            data_comment = client.query(
                TableName="Books", 
                KeyConditionExpression="id = :id AND rv_id > :rv_id", 
                ExpressionAttributeValues={
                    ":id": {"S": book['id']}, 
                    ":rv_id": {"N": "0"}
                }
            )
            format_data_comment = deserialize(data_comment['Items'])
            book["comments"] = format_data_comment
        except Exception as e:
            print(e)
            
    return {
        "statusCode": 200,
        "headers": {
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Methods": "GET,PUT,POST,DELETE, OPTIONS",
            "Access-Control-Allow-Headers": "Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method,X-Access-Token,XKey,Authorization"
        },
        "body": json.dumps(format_data_books, cls=DecimalEncoder)
    }
```
- The changed sources is:
```
    try:
        data_books = client.scan(
            TableName='Book',
            IndexName='name-index'
        )
    except Exception as e:
        print(e)
```
The table name has been changed from **Book** and added try-except to catch errors

8. Recall the API as in step 1, the error returned is **Internal server error**
9. To see the specific error we go back to the CloudWatch logs dashboard. Wait a moment for the log to finish recording. Then click on the latest log

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-8.png?featherlight=false&width=90pc)

10. Expand the error to see details
11. If you want the error returned to be the same as the error recorded in the log, add the following code to the except block when scanning the **Book** table


```
        return {
            "statusCode": 400,
            "headers": {
                "Content-Type": "application/json",
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET,PUT,POST,DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method,X-Access-Token,XKey,Authorization"
            },
            "body": str(e)
        }
```
12. Recall the API as in step 1, then the error returned will be the same as the error recorded in the log

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-9.png?featherlight=false&width=90pc)
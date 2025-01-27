---
title : "Gỡ lỗi với CloudWatch logs"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1. </b> "
---
1. Mở **Postnam** để gọi api 
- Thêm một tab mới
- Chọn phương thức **GET**
- Dán URL API đã ghi ở bước trước vào và thêm `books` ở cuối
- Ấn **Send**

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-1.png?featherlight=false&width=90pc)

- Sau khi hoàn thành dữ liệu của bảng **Books** được trả về

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-2.png?featherlight=false&width=90pc)

2. Mở bảng điều khiển của [AWS Lambda]()

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-3.png?featherlight=false&width=90pc)

3. Ấn chọn functions **books_list** 

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-4.png?featherlight=false&width=90pc)

4. Chọn tab **Monitor**
- Ấn **View logs CloudWatch**

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-5.png?featherlight=false&width=90pc)

5. Bạn sẽ thấy tất cả các log được lưu lại mỗi lần function **books_list** được thực hiện

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-6.png?featherlight=false&width=90pc)

6. Ấn vào log mới nhất

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-7.png?featherlight=false&width=90pc)

Bạn sẽ thấy function chạy bình thường và không có lỗi gì. Tiếp theo chúng ta sẽ sửa code để function chạy lỗi.

7. Sửa code như sau
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
- Đoạn bị thay đổi là:
```
    try:
        data_books = client.scan(
            TableName='Book',
            IndexName='name-index'
        )
    except Exception as e:
        print(e)
```
Tên bảng đã được thay đổi từ sang thành **Book** và thêm try-except để bắt lỗi

8. Gọi lại API như bước số 1, lỗi trả về là **Internal server error**
9. Để xem lỗi cụ thể chúng ta quay lại bảng điều khiển của CloudWatch logs. Đợi một lát để log ghi xong. Sau đó ấn vào log mới nhất
10. Mở rộng lỗi để xem chi tiết

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-8.png?featherlight=false&width=90pc)

11. Nếu bạn muốn lỗi trả về giống với lỗi được ghi trong log thì bạn thêm đoạn code sau vào except block khi scan bảng **Book**
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
12. Gọi lại API như bước số 1, khi đó lỗi trả về sẽ giống với lỗi được ghi trong log

![CreateRepository](/images/2-cloudwatch-monitor/2-1-cloudwatch-log-9.png?featherlight=false&width=90pc)

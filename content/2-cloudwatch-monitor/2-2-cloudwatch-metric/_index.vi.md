---
title : "Tạo số liệu tùy chỉnh"
date : 2025-02-11
weight : 2
chapter : false
pre : " <b> 2.2. </b> "
---

CloudWatch metric cung cấp một số số liệu cho các hàm Lambda như số lần hàm được thực thi, thời gian thực thi mỗi lần, tỷ lệ lỗi và số lần bị giới hạn. Để xem các số liệu của một hàm nhất định, chúng ta thực hiện các bước sau.

1. Mở [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Nhấn **Functions** trên menu bên trái.
    - Chọn hàm **books_list**.
      ![CloudWatchMetrics](/images/temp/1/5.png?width=90pc)

2. Tại trang **books_list**.
    - Nhấn tab **Monitor**.
    - Bạn có thể thấy các **CloudWatch metrics** được hiển thị.
      ![CloudWatchMetrics](/images/temp/1/12.png?width=90pc)

3. Tiếp theo, chúng ta sẽ tạo một số liệu tùy chỉnh mới để tổng hợp số lần truy cập vào DynamoDB bị lỗi. Tại trang **books_list**.
    - Nhấn tab **Code**.
    - Sao chép đoạn mã sau.

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

    - Nhấn nút **Deploy**.
      ![CloudWatchMetrics](/images/temp/1/13.png?width=90pc)

4. Tại trang **books_list**.
    - Nhấn tab **Configuration**.
    - Nhấn **Permissions** trên menu bên trái.
    - Nhấn **fcj-book-store-BooksListRole-...** tại **Role name**.
      ![CloudWatchMetrics](/images/temp/1/14.png?width=90pc)

5. Tại trang **fcj-book-store-BooksListRole-...**.
    - Nhấn tab **Permissions**.
    - Nhấn **+** tại **BooksListRolePolicy0** Policy name.
    - Nhấn nút **Edit**.
      ![CloudWatchMetrics](/images/temp/1/15.png?width=90pc)

6. Tại trang **Step 1: Modify permissions in BooksListRolePolicy0**.
    - Nhấn tab **JSON**.
    - Sao chép đoạn mã sau vào **Policy editor**.

      ```json
      {
          "Sid": "VisualEditor0",
          "Effect": "Allow",
          "Action": "cloudwatch:PutMetricData",
          "Resource": "*"
      },
      ```

      ![CloudWatchMetrics](/images/temp/1/16.png?width=90pc)
    - Cuộn xuống cuối trang và nhấn nút **Next**.
      ![CloudWatchMetrics](/images/temp/1/17.png?width=90pc)

7. Tại trang **Step 2: Review and save**.
    - Nhấn nút **Save changes**.
      ![CloudWatchMetrics](/images/temp/1/18.png?width=90pc)

8. Mở **Postman** để gọi lại API, lỗi trả về là **Internal server error**.
    ![CloudWatchMetrics](/images/temp/1/9.png?width=90pc)

9. Quay lại trang hàm Lambda **books_list**.
    - Nhấn tab **Monitor**.
    - Nhấn nút **View CloudWatch logs**.
      ![CloudWatchLog](/images/temp/1/6.png?width=90pc)

10. Tại trang **CloudWatch**.
    - Nhấn **All metrics** trên menu bên trái.
    - Nhấn **BooksList_Lambda** tại **Custom namespaces**.
      ![CloudWatchMetrics](/images/temp/1/19.png?width=90pc)
    - Tiếp theo, nhấn **env**.
      ![CloudWatchMetrics](/images/temp/1/20.png?width=90pc)
    - Nhấn **staging**.
    - Nhấn **Add to graph**.
      ![CloudWatchMetrics](/images/temp/1/21.png?width=90pc)
    - Sau khi biểu đồ được làm mới, di chuột qua điểm **Blue** trên biểu đồ.
    - Sau đó, bạn có thể thấy thông tin.
      ![CloudWatchMetrics](/images/temp/1/22.png?width=90pc)

Vậy là chúng ta đã tạo thành công một số liệu tùy chỉnh. Bước tiếp theo chúng ta sẽ sử dụng nó để tạo một CloudWatch Alarm.

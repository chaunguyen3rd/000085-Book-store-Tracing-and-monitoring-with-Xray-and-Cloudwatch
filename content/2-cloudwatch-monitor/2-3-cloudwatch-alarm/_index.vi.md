---
title : "Tạo cảnh báo với CloudWatch Alarm"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3. </b> "
---

1. Mở [AWS CloudWatch console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1).
    - Nhấp vào **In alarm** trên menu bên trái.
    - Nhấp vào nút **Create alarm**.
      ![CreateAlarm](/images/temp/1/23.png?width=90pc)

2. Tại trang **Step 1: Specify metric and conditions**.
    - Nhấp vào nút **Select metric**.
      ![CreateAlarm](/images/temp/1/24.png?width=90pc)
    - Tại cửa sổ **Select metric**.
      - Nhấp vào **BooksList_Lambda** tại **Custom namespaces**.
        ![CreateAlarm](/images/temp/1/25.png?width=90pc)
      - Tiếp theo, nhấp vào **env**.
        ![CreateAlarm](/images/temp/1/26.png?width=90pc)
      - Chọn **staging**.
      - Nhấp vào nút **Select metric**.
        ![CreateAlarm](/images/temp/1/27.png?width=90pc)
    - Chọn **Sum** tại **Statistic**.
    - Chọn **Static** tại **Threshold type**.
    - Chọn **Greater/Equal** tại **Whenever FailedConnectToDynamoDB is...**.
    - Nhập `2` tại **than...**.
    - Nhấp vào nút **Next**.
      ![CreateAlarm](/images/temp/1/28.png?width=90pc)

3. Tại trang **Step 2: Configure actions**.
    - Chọn **In alarm** tại **Alarm state trigger**.
    - Chọn **Create new topic** tại **Send a notification to the following SNS topic**.
    - Nhập `fcj_cloudwatch_alarm_topic` tại **Create a new topic…**.
    - Nhập email mà bạn muốn nhận thông báo.
    - Nhấp vào nút **Create topic**.
      ![CreateAlarm](/images/temp/1/29.png?width=90pc)

4. Mở email mà bạn đã chọn để nhận thông báo trước đó.
    - Nhấp vào **Confirm subscription**.
      ![CreateAlarm](/images/temp/1/30.png?width=90pc)
    - Sau đó bạn sẽ nhận được thông báo **Subscription confirmed**.
      ![CreateAlarm](/images/temp/1/31.png?width=90pc)

5. Quay lại trang **Step 2: Configure actions**.
    - Cuộn xuống dưới cùng và nhấp vào nút **Next**.
      ![CreateAlarm](/images/temp/1/32.png?width=90pc)

6. Tại trang **Step 3: Add name and description**.
    - Nhập `fcj-fail-connect-to-dynamodb-alarm` tại **Alarm name**.
    - Nhấp vào nút **Next**.
      ![CreateAlarm](/images/temp/1/33.png?width=90pc)

7. Tại trang **Step 4: Preview and create**.
    - Nhấp vào nút **Create alarm**.
      ![CreateAlarm](/images/temp/1/34.png?width=90pc)

8. Mở [AWS CloudWatch console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1).
    - Nhấp vào **All alarms** trên menu bên trái.
    - Bạn có thể thấy **fcj-fail-connect-to-dynamodb-alarm** mà chúng ta đã tạo ở bước trước.
      ![CreateAlarm](/images/temp/1/35.png?width=90pc)

9. Mở **Postman** để gọi lại API hai lần.
    ![CloudWatchMetrics](/images/temp/1/9.png?width=90pc)

10. Mở email mà bạn đã chọn để nhận thông báo trước đó.
    - Tìm email được gửi từ <no-reply@sns.amazonaws.com>.
      ![CloudWatchMetrics](/images/temp/1/36.png?width=90pc)

11. Quay lại [AWS CloudWatch console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1).
    - Nhấp vào **All alarms** trên menu bên trái.
    - Chọn **fcj-fail-connect-to-dynamodb-alarm** tại **Name**.
      ![CloudWatchMetrics](/images/temp/1/37.png?width=90pc)
    - Tại trang **fcj-fail-connect-to-dynamodb-alarm**, bạn có thể thấy biểu đồ của chỉ số được hiển thị.
      ![CloudWatchMetrics](/images/temp/1/38.png?width=90pc)

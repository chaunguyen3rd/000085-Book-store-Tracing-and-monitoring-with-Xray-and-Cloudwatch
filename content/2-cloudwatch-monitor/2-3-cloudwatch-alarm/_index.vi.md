---
title : "Tạo cảnh báo với CloudWatch Alarm"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3. </b> "
---
1. Mở bảng điều khiển của [CloudWatch]()
2. Mở rộng **Alarms** phía bên trái, ấn **In alarm**
3. Ấn **Create alarm**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-1.png?featherlight=false&width=90pc)

4. Ấn **Select metric**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-2.png?featherlight=false&width=90pc)

5. Chọn metric vừa tạo ở bước trước - **BooksList_Lambda**, sau đó ấn **env**, tiếp theo chọn **staging**
- Ấn **Select metric**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-3.png?featherlight=false&width=90pc)

6. Chọn **Sum** cho mục **Statistic**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-4.png?featherlight=false&width=90pc)

7. Thiếp lập điều kiện tại mục condition
- Chọn **Static** cho **Threshold type**
- Chọn **Greater/Equal** làm điều kiện cho cảnh báo
- Nhập **2** là giá trị của ngưỡng cảnh báo
8. Ấn **Next**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-5.png?featherlight=false&width=90pc)

9. Chọn **Create new topic**
10. Nhập tên topic
12. Nhập email mà bạn muốn nhận thông báo
13. Ấn **Create topic**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-6.png?featherlight=false&width=90pc)

14. Kéo xuống dưới, ấn **Next**
15. Nhập tên cho cảnh báo
- Ấn **Next**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-7.png?featherlight=false&width=90pc)

16. Kéo xuống cuối và ấn **Create alarm**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-8.png?featherlight=false&width=90pc)

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-9.png?featherlight=false&width=90pc)

17. Mở mail mà bạn đăng ký topic để xác nhận email. Ấn **Confirm subcription**.

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-10.png?featherlight=false&width=90pc)

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-11.png?featherlight=false&width=90pc)

17. Trở lại với màn hình của Postman, bạn hãy ấn **Send** hai lần để gọi API hai lần
18. Mở email và kiểm tra

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-12.png?featherlight=false&width=90pc)

19. Trở lại với bảng điều khiển của CloudWatch
20. Ấn **All alarms**, sau đó chọn cảnh báo mà bạn vừa tạo
21. Biểu đồ của metric được hiện thị
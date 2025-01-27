---
title : "Giám sát với X-ray"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3. </b> "
---
Trong phần này chúng ta sẽ kích hoạt X-ray cho Lambda function để track incoming và outgoing requests tới function, biết được từng đoạn của function tốn bao nhiêu thời gian. Khi đó chúng ta sẽ biết được function bị chậm ở chỗ nào và từ đó dễ dàng tối ưu hoá nó.

1. Mở bảng điều khiển của [AWS Lambda]()
2. Ấn vào function **book_delete**
3. Ấn tab **Configuration**
- Ấn **Monitoring and operations tools** ở menu phía bên trái
- Ấn **Edit**

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-1.png?featherlight=false&width=90pc)

- Ấn **Active tracing** ở mục AWS X-Ray
- Ấn **Save**

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-2.png?featherlight=false&width=90pc)

4. Gọi DELETE API bằng Postman
5. Mở bảng điều khiển của CloudWatch
- Mở rộng X-Ray traces
- Ấn **Traces**
- Kéo xuống cuối ấn vào trace đang hiện thị

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-11.png?featherlight=false&width=90pc)


- Initialization subsegment: đại diện cho giai đoạn init của vòng đời môi trường thực thi của Lambda. Trong giai đoạn này, Lambda tạo hoặc mở môi trường thực thi với các tài nguyên đã được cấu hình, tải xuống mã hàm và tất cả cá lớp, chạy runtime và khởi tạo hàm.
- Invocation subsegment: đại diện cho giai đoạn Lambda gọi trình xử lý hàm. Điều này bắt đầu với thời gian chạy và đăng ký tiện ích mở rộng và nó kết thúc khi thời gian chạy đã sẵn sàng để gửi phản hồi.
- Overhead subsegment: đại diện cho giai đoạn xảy ra giữa thời gian mà runtime gửi phản hồi và tín hiệu cho lần gọi tiếp theo. Trong thời gian này, runtime kết thúc tất cả các tác vụ liên quan đến một lệnh gọi và chuẩn bị đóng băng hộp cát.

6. Để patch tất cả các thư viện được sử dụng trong function chúng ta thêm đoạn code sau vào đầu trong máy của bạn
```
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.core import patch_all

patch_all()
```

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-4.png?featherlight=false&width=90pc)

7. Chạy các câu lệnh sau tại thư mục **book_delete**
```
pip install --target ./package aws_xray_sdk
cd package
zip -r ../deployment-package.zip .
cd ..
zip -g deployment-package.zip book_delete.py
```
8. Quay lại với bảng điều khiển của function **book_delete**
- Ấn tab **Code**
- Ấn **Upload from**, chọn **.zip file**
- Ấn **Upload**, sau đó chọn tệp **deployment-package.zip** mà bạn vừa tạo
- Ấn **Save**

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-5.png?featherlight=false&width=90pc)

9. Gọi DELETE API bằng Postman

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-6.png?featherlight=false&width=90pc)

10. Điều hướng đến bảng điều khiển của CloudWatch
11. Ấn vào trace mới nhất

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-7.png?featherlight=false&width=90pc)

12. Bạn sẽ thấy các thông tin cụ thể hơn so với trace trước

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-8.png?featherlight=false&width=90pc)

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-9.png?featherlight=false&width=90pc)

---
title : "Serverless - Giám sát Lambda với CloudWatch và X-Ray"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---

# Serverless - Giám sát ứng dụng Serverless với CloudWatch và X-Ray

#### Tổng quan

Giám sát và quan sát ứng dụng là một bước quan trọng trong việc triển khai ứng dụng để đảm bảo rằng tất cả các dịch vụ của ứng dụng đang hoạt động đúng cách và có khả năng xử lý trong trường hợp xảy ra sự cố. AWS cung cấp các công cụ giúp chúng ta làm điều đó như AWS CloudWatch, AWS X-Ray và AWS CloudTrail. Trong bài viết này, chúng ta sẽ tìm hiểu cách gỡ lỗi AWS Lambda thông qua AWS CloudWatch, giám sát Lambda bằng các chỉ số tích hợp sẵn của CloudWatch hoặc các chỉ số tùy chỉnh mà chúng ta định nghĩa, và cách theo dõi API bằng AWS X-Ray.

#### Amazon CloudWatch

Amazon CloudWatch giám sát các tài nguyên AWS mà bạn sử dụng và các ứng dụng bạn chạy trong thời gian thực. Sử dụng CloudWatch để thu thập và theo dõi các chỉ số, là các biến số mà bạn có thể đo lường cho tài nguyên và ứng dụng của mình. Tất cả các dịch vụ đám mây đang được sử dụng sẽ cung cấp các chỉ số cho CloudWatch và tự động hiển thị chúng khi truy cập bảng điều khiển CloudWatch. CloudWatch cũng cung cấp các dịch vụ sau:

- Metrics: một tập hợp các chỉ số tích hợp vào các dịch vụ AWS đang được sử dụng
- Logs: thu thập và lưu trữ các tệp nhật ký
- Events: gửi thông báo để phản hồi các sự kiện
- Alarms: đặt ngưỡng kích hoạt (cảnh báo) để kích hoạt một hành động

#### AWS X-ray

X-ray giúp các nhà phát triển phân tích và gỡ lỗi các ứng dụng sản xuất và phân tán, chẳng hạn như những ứng dụng được xây dựng bằng kiến trúc microservices. Với X-ray, bạn có thể hiểu cách ứng dụng của bạn và các dịch vụ cơ bản của nó hoạt động để xác định và khắc phục nguyên nhân gốc rễ của các vấn đề về hiệu suất và sự cố. X-ray cung cấp cái nhìn toàn diện về các yêu cầu khi chúng di chuyển qua ứng dụng của bạn và hiển thị bản đồ các thành phần cơ bản của ứng dụng của bạn. Bạn có thể sử dụng X-ray để phân tích cả ứng dụng phát triển và sản xuất, từ các ứng dụng đơn giản 3 tầng đến các ứng dụng microservices phức tạp bao gồm hàng ngàn dịch vụ.

#### Nội dung

1. [Chuẩn bị](1-preparation/)
2. [Giám sát với CloudWatch](2-build-sam-pipeline/)
3. [Giám sát với X-ray](3-build-frontend-pipeline/)
4. [Dọn dẹp tài nguyên](4-cleanup)

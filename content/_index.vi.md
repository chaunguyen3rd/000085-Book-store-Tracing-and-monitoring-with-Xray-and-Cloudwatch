---
title : "Serverless - Giám sát Lambda với CloudWatch và X-Ray"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Serverless - Giám sát Lambda với CloudWatch và X-Ray

#### Tổng quan
Việc theo dõi và quan sát ứng dụng là một bước quan trọng trong triển khai ứng dụng để đảm bảo rằng tất cả các dịch vụ của ứng dụng hoạt động tốt và có khả năng xử lý trong trường hợp xảy ra lỗi. AWS cung cấp một là công cụ giúp chúng ta thực hiện việc đó như AWS CloudWatch, AWS X-Ray, AWS CloudTrail. Bài này chúng ta sẽ tìm hiểu cách gỡ lỗi AWS Lambda thông qua AWS CloudWatch, monitoring Lambda dùng metrics có sẵn của CloudWatch hoặc custom metrics mà ta tự định nghĩa, và cách để tracing API dùng AWS X-Ray.

#### Amazon CloudWatch
Amazon CloudWatch giám sát tài nguyên AWS mà bạn sử dụng và các ứng dụng bạn chạy trong thời gian thực. Sử dụng CloudWatch để thu thập và theo dõi các chỉ số, là những biến số mà bạn có thể đo lường cho tài nguyên và ứng dụng của mình. Tất các dịch vụ cloud đang được sử dụng sẽ cung cấp metrics cho CloudWatch và tự động hiện thị khi truy cập vào bảng điều khiển của CloudWatch. CloudWatch cũng cấp các dịch vụ sau:
- Metrics: tập hợp các chỉ số được tích hợp vào các dịch vụ AWS mà đang được sử dụngc
- Logs: tập hợp và lưu trữ các tệp log 
- Events: gửi thông thông báo để phản hồi lại sự kiện
- Alarms: đặt các ngưỡng kích hoạt (cảnh báo) để kích hoạt một hành động

#### AWS X-ray
X-ray giúp các nhà phát triển phân tích và gỡ lỗi production, ứng dụng phân tán, chẳng hạn như ứng dụng được xây dựng bằng kiến trúc microservices. Với X-ray, bạn có thể hiểu ứng dụng của mình và các dịch vụ cơ bản của nó hoạt động như thế nào để xác định và khắc phục nguyên nhân gốc rễ của vấn đề và lỗi về hiệu xuất. X-ray cung cấp chế độ xem từ đấu đến cuối của các yêu cầu khi chúng di chuyển qua ứng dụng của bạn và hiện thị bản đồ các thành phần cơ bản của ứng dụng. Bạn có thể sử dụng X-ray để phân tích cả các ứng dụng development và production, từ các dụng dụng 3 cấp đơn giản đến ứng dụng microservices phức tạp gồm hàng nghìn dịch vụ.

#### Nội dung

 1. [Chuẩn bị](1-preparation/)
 2. [Giám sát với CloudWatch](2-build-sam-pipeline/)
 3. [Giám sát với X-ray](3-build-frontend-pipeline/)
 4. [Dọn dẹp tài nguyên](4-cleanup)

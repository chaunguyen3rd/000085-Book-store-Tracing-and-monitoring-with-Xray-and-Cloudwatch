---
title : "Chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Trước khi thực hiện nội dung chính của workshop này, chúng ta chuẩn bị các dịch vụ và dữ liệu cho ứng dụng.
1. Tải source code của sam project dưới đây

{{%attachments title="SAM source" pattern=".*\.(zip)$"/%}}

2. Chạy câu lệnh dưới đây để triển khai sam
```
sam build
sam deploy --guided
```

Chúng ta đã chuẩn bị xong source cần thiết cho các bước tiếp theo.

---
title : "Chuẩn bị"
date : 2025-02-11
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

Trước khi thực hiện nội dung chính của hội thảo này, chúng ta chuẩn bị các dịch vụ và dữ liệu cho ứng dụng.

1. Tải mã nguồn của dự án **SAM** dưới đây.
    {{%attachments title="Mã nguồn SAM" pattern=".*\.(zip)$"/%}}

2. Giải nén và đi đến thư mục gốc của dự án **fcj-book-store-sam-ws8**. Chạy các lệnh dưới đây.
{{% notice note %}}
Đảm bảo bạn đã cài đặt AWS CLI và SAM CLI trên máy của mình, cấu hình thông tin xác thực AWS trước khi chạy các lệnh.
{{% /notice %}}

    ```bash
    sam build
    sam validate
    sam deploy --guided
    ```

3. Nhập nội dung sau. Để mặc định.
    - Stack Name []: `fcj-book-store`
    - AWS Region []: `us-east-1`
    - Confirm changes before deploy [Y/n]: y
    - Allow SAM CLI IAM role creation [Y/n]: y
    - Disable rollback [y/N]: n
    - Save arguments to configuration file [Y/n]: y
    - Sau khi tạo thành công, ghi lại giá trị của **ApiUrl** outputs.
      ![Chuẩn bị](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/1.png?width=90pc)

4. Để thêm dữ liệu vào bảng, bạn có thể tải tệp dưới đây:
{{%attachments title="Dữ liệu" pattern=".*\.(json)$"/%}}

5. Mở tệp này, thay thế tất cả **`<AWS-REGION>`** bằng khu vực nơi bạn đã tạo bucket S3 **book-image-resize-shop-by-myself**, ví dụ: **us-east-1**.
  ![Chuẩn bị](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/2.png?width=90pc)

6. Chạy lệnh sau trong thư mục nơi bạn lưu dynamoDB.json

    ```bash
    aws dynamodb batch-write-item --request-items file://dynamoDB.json
    ```

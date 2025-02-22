---
title : "Theo dõi với X-ray"
date : 2025-02-11
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

Trong phần này, chúng ta sẽ bật X-ray cho hàm Lambda để theo dõi các yêu cầu đến và đi từ hàm, biết được mỗi đoạn của hàm mất bao nhiêu thời gian. Sau đó, chúng ta sẽ biết được hàm chậm ở đâu và từ đó dễ dàng tối ưu hóa nó.

1. Mở [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Nhấp vào **Functions** trên menu bên trái.
    - Chọn hàm **book_delete**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/39.png?width=90pc)

2. Tại trang **book_delete**.
    - Nhấp vào tab **Configuration**.
    - Nhấp vào **Monitoring and operations tools** trên menu bên trái.
    - Nhấp vào nút **Edit**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/40.png?width=90pc)

3. Tại trang **Edit monitoring tools**.
    - Chọn **Enable** tại **Lambda service traces**.
    - Nhấp vào nút **Save**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/41.png?width=90pc)

4. Mở **Postman** để gọi API.
    - Nhấp vào **+** để thêm một tab mới.
    - Chọn phương thức **DELETE**.
    - Nhập URL của API xóa với **book_id**, ví dụ là `2`.
    - Nhấp vào **Send**.
    - Sau khi hoàn thành, sách với id `2` sẽ bị xóa.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/42.png?width=90pc)

5. Quay lại trang **book_delete**.
    - Nhấp vào tab **Monitor**.
    - Nhấp vào nút **View X-Ray traces**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/43.png?width=90pc)

6. Tại trang **CloudWatch**.
    - Nhấp vào **Traces** trên menu bên trái.
    - Nhấp vào nút **Run query**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/44.png?width=90pc)
    - Sau đó, cuộn xuống và chọn dấu vết hiện tại với trạng thái **OK**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/45.png?width=90pc)

7. Tại trang **Trace 1-...**. Bạn có thể thấy các phân đoạn con.
    - Phân đoạn con **Initialization**: Đại diện cho giai đoạn khởi tạo của vòng đời môi trường thực thi Lambda. Trong giai đoạn này, Lambda tạo hoặc mở một môi trường thực thi với các tài nguyên đã cấu hình, tải xuống mã hàm và tất cả các lớp, chạy runtime và khởi tạo hàm.
    - Phân đoạn con **Invocation**: Đại diện cho giai đoạn khi Lambda gọi hàm handler. Điều này bắt đầu với runtime và đăng ký phần mở rộng và kết thúc khi runtime sẵn sàng gửi phản hồi.
    - Phân đoạn con **Overhead**: Đại diện cho khoảng thời gian xảy ra giữa thời điểm runtime gửi phản hồi và tín hiệu cho lần gọi tiếp theo. Trong thời gian này, runtime hoàn thành tất cả các tác vụ liên quan đến một lần gọi và chuẩn bị đóng băng sandbox.
      ![XrayTrace](/images/temp/1/54.png?width=90pc)

8. Đi đến thư mục gốc của dự án **fcj-book-store-sam-ws8**. Mở thư mục **fcj-book-store-sam-ws8/fcj-book-shop/book_delete**.
    - Tạo một tệp có tên `requirements.txt` với nội dung dưới đây.

      ```txt
      aws_xray_sdk
      ```

      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/46.png?width=90pc)
    - Mở tệp **fcj-book-store-sam-ws8/fcj-book-shop/book_delete/book_delete.py** và sao chép nội dung dưới đây vào tệp đó.

        ```py
        from aws_xray_sdk.core import patch_all

        patch_all()
        ```

        ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/47.png?width=90pc)
    - Mở terminal của bạn và chạy các lệnh dưới đây tại thư mục gốc của dự án **fcj-book-store-sam-ws8**.

      ```bash
      sam build
      sam validate
      sam deploy
      ```

      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/48.png?width=90pc)

9. Quay lại trang **book_delete**.
    - Nhấp vào tab **Code**.
    - Kiểm tra xem hàm **book_delete** đã được cập nhật chưa.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/49.png?width=90pc)

10. Mở **Postman** để gọi lại API **DELETE** với id `4`.
    ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/50.png?width=90pc)

11. Quay lại trang **CloudWatch**.
    - Nhấp vào **Traces** trên menu bên trái.
    - Nhấp vào nút **Run query**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/44.png?width=90pc)
    - Sau đó, cuộn xuống và chọn dấu vết hiện tại với trạng thái **OK**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/51.png?width=90pc)

12. Tại trang **Trace 1-...**. Bạn có thể thấy thông tin chi tiết hơn so với dấu vết trước đó.
    ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/52.png?width=90pc)
    ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/53.png?width=90pc)

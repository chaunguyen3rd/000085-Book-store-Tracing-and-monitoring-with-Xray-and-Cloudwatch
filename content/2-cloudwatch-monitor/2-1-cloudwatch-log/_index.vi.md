---
title : "Gỡ lỗi với nhật ký CloudWatch"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1. </b> "
---

1. Mở **Postman** để gọi API.
    - Nhấn **+** để thêm một tab mới.
    - Chọn phương thức **GET**.
    - Nhập URL của API liệt kê đã được ghi lại từ bước trước.
    - Nhấn **Send**.
      ![CloudWatchLog](/images/temp/1/3.png?width=90pc)
    - Sau khi hoàn thành, dữ liệu của bảng **Books** sẽ được trả về.
      ![CloudWatchLog](/images/temp/1/4.png?width=90pc)

2. Mở [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Nhấn **Functions** trên menu bên trái.
    - Chọn hàm **books_list**.
      ![CloudWatchLog](/images/temp/1/5.png?width=90pc)

3. Tại trang **books_list**.
    - Nhấn tab **Monitor**.
    - Nhấn nút **View CloudWatch logs**.
      ![CloudWatchLog](/images/temp/1/6.png?width=90pc)

4. Tại trang **/aws/lambda/books_list**.
    - Bạn sẽ thấy tất cả các nhật ký được lưu mỗi khi hàm **books_list** được thực thi.
      ![CloudWatchLog](/images/temp/1/7.png?width=90pc)

5. Tiếp theo, chúng ta sẽ sửa mã để làm cho hàm chạy thất bại. Sao chép đoạn mã sau.

    ```python
    books_data = table.scan(
        TableName='Book',
        IndexName=secondary_index
    )
    ```

    - Quay lại trang **books_list**.
      - Nhấn tab **Code**.
      - Thay đổi mã như đoạn mã đã sao chép ở trên.
      - Nhấn nút **Deploy**.
        ![CloudWatchLog](/images/temp/1/8.png?width=90pc)

6. Sau khi **books_list** được triển khai thành công. Gọi lại API như ở bước 1, lỗi trả về là **Internal server error**.
    ![CloudWatchLog](/images/temp/1/9.png?width=90pc)

7. Quay lại trang **/aws/lambda/books_list** CloudWatch Logs.
    - Nhấn biểu tượng **Refresh**.
    - Nhấn vào nhật ký mới nhất tại **Log streams**.
      ![CloudWatchLog](/images/temp/1/10.png?width=90pc)

8. Tại trang nhật ký chi tiết. Bạn có thể thấy lỗi. Lỗi là do chúng ta đã thay đổi **TableName='Books'** thành **TableName='Book'** (**Books** là tên của bảng DynamoDB mà chúng ta vừa tạo trước đó).
    ![CloudWatchLog](/images/temp/1/11.png?width=90pc)

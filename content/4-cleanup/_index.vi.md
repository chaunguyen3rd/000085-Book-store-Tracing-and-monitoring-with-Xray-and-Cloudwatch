---
title : "Dọn dẹp"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

1. Làm trống S3 bucket.
    - Mở [AWS S3 console](https://s3.console.aws.amazon.com/s3/buckets?region=us-east-1).
    - Chọn **aws-sam-cli-managed-default-**.
    - Nhấp vào **Empty**.
    - Nhập ``permanently delete``.
    - Nhấp vào **Empty**.

2. Xóa CloudFormation stacks.
    - Thực hiện lệnh dưới đây để xóa ứng dụng AWS SAM.

      ```bash
      sam delete --stack-name fcj-book-store
      sam delete --stack-name aws-sam-cli-managed-default
      ```

    - Nếu bạn gặp vấn đề khi xóa bằng lệnh. Mở [AWS Cloudformation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/getting-started). Sau đó, xóa tất cả các stacks liên quan đến workshop này.

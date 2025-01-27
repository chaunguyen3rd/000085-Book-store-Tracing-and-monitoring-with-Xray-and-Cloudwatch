---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
1. Làm rỗng S3 bucket
- Mở bảng điều khiển của [AWS S3](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1)
- Chọn **fcjdmsstore**
- Ấn **Empty**
- Nhập **permanently delete**
- Ấn **Empty**
- Làm tương tự với bucket bắt đầu bằng **aws-sam-cli-managed-default-** và bucket **fcjdmswebstore**
2. Xoá stack của CloudFormation
```
sam delete --stack-name fcjdmssam
sam delete --stack-name aws-sam-cli-managed-dev-pipeline-resources
```
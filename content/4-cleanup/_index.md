---
title : "Cleanup"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
1. Empty S3 bucket
- Open [AWS S3 console](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1)
- Select **fcj-book-store**
- Click **Empty**
- Enter **permanently delete**
- Click **Empty**
- Do the same for bucket starting with **aws-sam-cli-managed-default-** and **book-image-resize-store** bucket
2. Delete CloudFormation stacks:
```
sam delete --stack-name fcjdmssam
sam delete --stack-name aws-sam-cli-managed-dev-pipeline-resourcest
```
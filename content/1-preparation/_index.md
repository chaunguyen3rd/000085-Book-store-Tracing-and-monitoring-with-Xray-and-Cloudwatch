---
title : "Preparing"
date : 2025-02-11
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

Before doing the main content of this workshop, we prepare the services and data for the application.

1. Download the source code of the **SAM** project below.
    {{%attachments title="SAM source" pattern=".*\.(zip)$"/%}}

2. Unzip and go to the root directory of **fcj-book-store-sam-ws8** project. Run the below commands.
{{% notice note %}}
Ensure you have the AWS CLI and SAM CLI installed on your machine, configure AWS credentials before running the commands.
{{% /notice %}}

    ```bash
    sam build
    sam validate
    sam deploy --guided
    ```

3. Enter the following content. Leave as default.
    - Stack Name []: `fcj-book-store`
    - AWS Region []: `us-east-1`
    - Confirm changes before deploy [Y/n]: y
    - Allow SAM CLI IAM role creation [Y/n]: y
    - Disable rollback [y/N]: n
    - Save arguments to configuration file [Y/n]: y
    - After successful creation, record the value of the **ApiUrl** outputs.
      ![Preparation](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/1.png?width=90pc)

4. To add data to the table, you can download the file below:
{{%attachments title="Data" pattern=".*\.(json)$"/%}}

5. Open this file, replace all **`<AWS-REGION>`** with the region where you created the S3 **book-image-resize-shop-by-myself** bucket, for example: **us-east-1**.
  ![Preparation](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/2.png?width=90pc)

6. Run the following command in the directory where you save the dynamoDB.json

    ```bash
    aws dynamodb batch-write-item --request-items file://dynamoDB.json
    ```

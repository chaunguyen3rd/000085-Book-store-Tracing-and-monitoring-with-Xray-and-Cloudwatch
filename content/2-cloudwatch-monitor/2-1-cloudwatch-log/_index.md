---
title : "Debug with CloudWatch logs"
date : 2025-02-11
weight : 1
chapter : false
pre : " <b> 2.1. </b> "
---

1. Open **Postman** to call api.
    - Click **+** to add a new tab.
    - Select **GET** method.
    - Enter URL of the listing API that recorded from the previous step.
    - Click **Send**.
      ![CloudWatchLog](/images/temp/1/3.png?width=90pc)
    - After completing, the data of the **Books** table is returned.
      ![CloudWatchLog](/images/temp/1/4.png?width=90pc)

2. Open [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Click **Functions** on the left menu.
    - Choose the **books_list** function.
      ![CloudWatchLog](/images/temp/1/5.png?width=90pc)

3. At **books_list** page.
    - Click the **Monitor** tab.
    - Click the **View CloudWatch logs** button.
      ![CloudWatchLog](/images/temp/1/6.png?width=90pc)

4. At **/aws/lambda/books_list** page.
    - You will see all the logs saved each time the function **books_list** is executed.
      ![CloudWatchLog](/images/temp/1/7.png?width=90pc)

5. Next, we will fix the code to make the function run failed. Copy the following code.

    ```python
    books_data = table.scan(
        TableName='Book',
        IndexName=secondary_index
    )
    ```

    - Back to **books_list** page.
      - Click the **Code** tab.
      - Change the code as the copied one above.
      - Click the **Deploy** button.
        ![CloudWatchLog](/images/temp/1/8.png?width=90pc)

6. After **books_list** is deployed successfully. Recall the API as in step 1, the error returned is **Internal server error**.
    ![CloudWatchLog](/images/temp/1/9.png?width=90pc)

7. Back to **/aws/lambda/books_list** CloudWatch Logs page.
    - Click the **Refresh** icon.
    - Click the latest log at the **Log streams**.
      ![CloudWatchLog](/images/temp/1/10.png?width=90pc)

8. At the detailed log page. You could see the error. The error is because we changed **TableName='Books'** to **TableName='Book'** (**Books** is the name of the DynamoDB table that we just created before).
    ![CloudWatchLog](/images/temp/1/11.png?width=90pc)

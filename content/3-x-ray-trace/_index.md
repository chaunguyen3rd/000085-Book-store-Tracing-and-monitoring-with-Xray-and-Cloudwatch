---
title : "Tracing with X-ray"
date : 2025-02-11
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

In this section we will enable X-ray for the Lambda function to track incoming and outgoing requests to the function, knowing how much time each segment of the function takes. Then we will know where the function is slow and from there easily optimize it.

1. Open [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Click **Functions** on the left menu.
    - Choose the **book_delete** function.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/39.png?width=90pc)

2. At **book_delete** page.
    - Click the **Configuration** tab.
    - Click the **Monitoring and operations tools** on the lef menu.
    - Click the **Edit** button.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/40.png?width=90pc)

3. At **Edit monitoring tools** page.
    - Check on the **Enable** at **Lambda service traces**.
    - Click the **Save** button.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/41.png?width=90pc)

4. Open **Postman** to call api.
    - Click **+** to add a new tab.
    - Select **DELETE** method.
    - Enter URL of the deleting API with **book_id**, for example is `2`.
    - Click **Send**.
    - After completing, the book with id `2` is deleted.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/42.png?width=90pc)

5. Back to **book_delete** page.
    - Click the **Monitor** tab.
    - Click the **View X-Ray traces** button.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/43.png?width=90pc)

6. At **CloudWatch** page.
    - Click the **Traces** on the left menu.
    - Click the **Run query** button.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/44.png?width=90pc)
    - Then, scroll down and select the current displaced trace with status **OK**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/45.png?width=90pc)

7. At **Trace 1-...** page. You can see the subsegment.
    - **Initialization** subsegment: Represents the init phase of the Lambda execution environment lifecycle. During this phase, Lambda creates or opens an execution environment with configured resources, downloads the function code and all classes, runs the runtime, and initializes the function.
    - **Invocation** subsegment: Represents the stage when Lambda calls the function handler. This starts with the runtime and registers the extension and it ends when the runtime is ready to send a response.
    - **Overhead** subsegment: Represents the period that occurs between the time that the runtime sends the response and signal for the next call. During this time, the runtime finishes all tasks associated with an invocation and prepares to freeze the sandbox.
      ![XrayTrace](/images/temp/1/54.png?width=90pc)

8. Go to the root directory of **fcj-book-store-sam-ws8** project. Open the **fcj-book-store-sam-ws8/fcj-book-shop/book_delete** directory.
    - Create a file called `requirements.txt` with the below content.

      ```txt
      aws_xray_sdk
      ```

      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/46.png?width=90pc)
    - Open the **fcj-book-store-sam-ws8/fcj-book-shop/book_delete/book_delete.py** and copy the below content to that file.

        ```py
        from aws_xray_sdk.core import patch_all

        patch_all()
        ```

        ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/47.png?width=90pc)
    - Open your terminal and run the below commands at the root directory of **fcj-book-store-sam-ws8** project.

      ```bash
      sam build
      sam validate
      sam deploy
      ```

      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/48.png?width=90pc)

9. Back to **book_delete** page.
    - Click the **Code** tab.
    - Check if the **book_delete** function is updated.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/49.png?width=90pc)

10. Open **Postman** to recall the **DELETE** api with the id `4`.
    ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/50.png?width=90pc)

11. Back to **CloudWatch** page.
    - Click the **Traces** on the left menu.
    - Click the **Run query** button.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/44.png?width=90pc)
    - Then, scroll down and select the current displaced trace with status **OK**.
      ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/51.png?width=90pc)

12. At **Trace 1-...** page. You can see the more specific information than in the previous trace.
    ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/52.png?width=90pc)
    ![XrayTrace](https://chaunguyen3rd.github.io/000085-Book-store-Tracing-and-monitoring-with-Xray-and-Cloudwatch/images/temp/1/53.png?width=90pc)

---
title : "Tracing with X-ray"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

In this section we will enable X-ray for the Lambda function to track incoming and outgoing requests to the function, knowing how much time each segment of the function takes. Then we will know where the function is slow and from there easily optimize it.

1. Open [AWS Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
    - Click **Functions** on the left menu.
    - Choose the **book_delete** function.
      ![XrayTrace](/images/temp/1/39.png?width=90pc)

2. At **book_delete** page.
    - Click the **Configuration** tab.
    - Click the **Monitoring and operations tools** on the lef menu.
    - Click the **Edit** button.
      ![XrayTrace](/images/temp/1/40.png?width=90pc)

3. At **Edit monitoring tools** page.
    - Check on the **Enable** at **Lambda service traces**.
    - Click the **Save** button.
      ![XrayTrace](/images/temp/1/41.png?width=90pc)

4. Open **Postman** to call api.
    - Click **+** to add a new tab.
    - Select **DELETE** method.
    - Enter URL of the deleting API with **book_id**, for example is `2`.
    - Click **Send**.
    - After completing, the book with id `2` is deleted.
      ![XrayTrace](/images/temp/1/42.png?width=90pc)

5. Back to **book_delete** page.
    - Click the **Monitor** tab.
    - Click the **View X-Ray traces** button.
      ![XrayTrace](/images/temp/1/43.png?width=90pc)

6. At **CloudWatch** page.
    - Click the **Traces** on the left menu.
    - Click the **Run query** button.
      ![XrayTrace](/images/temp/1/44.png?width=90pc)
    - Then, scroll down and select the current displaced trace with status **OK**.
      ![XrayTrace](/images/temp/1/45.png?width=90pc)

7. At **Trace 1-...** page.
    - Initialization subsegment: Represents the init phase of the Lambda execution environment lifecycle. During this phase, Lambda creates or opens an execution environment with configured resources, downloads the function code and all classes, runs the runtime, and initializes the function.
    - Invocation subsegment: represents the stage when Lambda calls the function handler. This starts with the runtime and registers the extension and it ends when the runtime is ready to send a response.
    - Overhead subsegment: represents the period that occurs between the time that the runtime sends the response and signal for the next call. During this time, the runtime finishes all tasks associated with an invocation and prepares to freeze the sandbox.

8. To patch all the libraries used in the function we add the following code at the beginning of your machine
```
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.core import patch_all

patch_all()
```

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-4.png?featherlight=false&width=90pc)

7. Run the following commands in the **book_delete** directory
```
pip install --target ./package aws_xray_sdk
cd package
zip -r ../deployment-package.zip .
cd ..
zip -g deployment-package.zip book_delete.py
```
8. Return to the function panel **book_delete**
- Press tab **Code**
- Click **Upload from**, select **.zip file**
- Press **Upload**, then select the **deployment-package.zip** file you just created
- Press **Save**

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-5.png?featherlight=false&width=90pc)

9. Call DELETE API with Postman

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-6.png?featherlight=false&width=90pc)

10. Navigate to the CloudWatch dashboard
11. Click on the latest trace

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-7.png?featherlight=false&width=90pc)

12. You will see more specific information than in the previous trace

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-8.png?featherlight=false&width=90pc)

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-9.png?featherlight=false&width=90pc)
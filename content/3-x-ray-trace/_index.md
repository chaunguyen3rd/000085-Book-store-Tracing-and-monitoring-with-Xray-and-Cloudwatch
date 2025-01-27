---
title : "Tracing with X-ray"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---
In this section we will enable X-ray for the Lambda function to track incoming and outgoing requests to the function, knowing how much time each segment of the function takes. Then we will know where the function is slow and from there easily optimize it.

1. Open console of [AWS Lambda]()
2. Click the function **book_delete**
3. Click the **Configure** tab
- Click **Monitoring and operations tools** in the menu on the left
- Press **Edit**

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-1.png?featherlight=false&width=90pc)

- Click **Active tracing** in the AWS X-Ray section
- Press **Save**

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-2.png?featherlight=false&width=90pc)

4. Call DELETE API with Postman
5. Open the CloudWatch dashboard
- Expanded X-Ray traces
- Press **Traces**
- Scroll to the bottom and click on the currently displayed trace

![CreateAlarm](/images/3-x-ray-trace/3-x-ray-trace-11.png?featherlight=false&width=90pc)

- Initialization subsegment: represents the init phase of the Lambda execution environment lifecycle. During this phase, Lambda creates or opens an execution environment with configured resources, downloads the function code and all classes, runs the runtime, and initializes the function.
- Invocation subsegment: represents the stage when Lambda calls the function handler. This starts with the runtime and registers the extension and it ends when the runtime is ready to send a response.
- Overhead subsegment: represents the period that occurs between the time that the runtime sends the response and signal for the next call. During this time, the runtime finishes all tasks associated with an invocation and prepares to freeze the sandbox.

6. To patch all the libraries used in the function we add the following code at the beginning of your machine
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
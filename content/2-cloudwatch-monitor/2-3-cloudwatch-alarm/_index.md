---
title : "Creating Alerts with CloudWatch Alarm"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3. </b> "
---
1. Open the dashboard of [CloudWatch]()
2. Expand **Alarms** on the left side, press **In alarm**
3. Press **Create alarm**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-1.png?featherlight=false&width=90pc)

4. Press **Select metric**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-2.png?featherlight=false&width=90pc)

5. Select the metric created in the previous step - **BooksList_Lambda**, then press **env**, then select **staging**
- Press **Select metric**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-3.png?featherlight=false&width=90pc)

6. Select **Sum** for **Statistic**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-4.png?featherlight=false&width=90pc)

7. Set the condition in the item condition
- Select **Static** for **Threshold type**
- Select **Greater/Equal** as the warning condition
- Enter **2** as the value of the alarm threshold
8. Click **Next**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-5.png?featherlight=false&width=90pc)

9. Select **Create new topic**
10. Enter the topic name
12. Enter the email you want to receive notifications for
13. Click **Create topic**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-6.png?featherlight=false&width=90pc)

14. Scroll down, click **Next**
15. Enter a name for the alert
- Click **Next**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-7.png?featherlight=false&width=90pc)

16. Scroll to the bottom and press **Create alarm**

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-8.png?featherlight=false&width=90pc)

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-9.png?featherlight=false&width=90pc)

17. Back to Postman's screen, press **Send** twice to call the API twice
18. Open email and check

![CreateAlarm](/images/2-cloudwatch-monitor/2-3-cloudwatch-alarm-12.png?featherlight=false&width=90pc)

19. Back to the CloudWatch dashboard
20. Click **All alarms**, then select the alarm you just created
21. The histogram of the metric is displayed
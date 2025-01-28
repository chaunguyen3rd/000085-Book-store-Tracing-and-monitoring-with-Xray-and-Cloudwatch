---
title : "Creating Alerts with CloudWatch Alarm"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3. </b> "
---

1. Open [AWS CloudWatch console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1).
    - Click the **In alarm** on the left menu.
    - Click the **Create alarm** button.
      ![CreateAlarm](/images/temp/1/23.png?width=90pc)

2. At **Step 1: Specify metric and conditions** page.
    - Click the **Select metric** button.
      ![CreateAlarm](/images/temp/1/24.png?width=90pc)
    - At **Select metric** popup.
      - Click the **BooksList_Lambda** at the **Custom namespaces**.
        ![CreateAlarm](/images/temp/1/25.png?width=90pc)
      - Next, click the **env**.
        ![CreateAlarm](/images/temp/1/26.png?width=90pc)
      - Check the **staging**.
      - Click the **Select metric** button.
        ![CreateAlarm](/images/temp/1/27.png?width=90pc)
    - Choose the **Sum** at **Statistic**.
    - Choose the **Static** at **Threshold type**.
    - Choose the **Greater/Equal** at **Whenever FailedConnectToDynamoDB is...**.
    - Enter `2` at **than...**.
    - Click the **Next** button.
      ![CreateAlarm](/images/temp/1/28.png?width=90pc)

3. At **Step 2: Configure actions** page.
    - Choose the **In alarm** at **Alarm state trigger**.
    - Choose the **Create new topic** at **Send a notification to the following SNS topic**.
    - Enter `fcj_cloudwatch_alarm_topic` at **Create a new topic…**.
    - Enter the email that you want to receive the notification.
    - Click the **Create topic** button.
      ![CreateAlarm](/images/temp/1/29.png?width=90pc)

4. Open the email that you chose to receive the notification before.
    - Click the **Confirm subscription**.
      ![CreateAlarm](/images/temp/1/30.png?width=90pc)
    - Then you will receive the **Subscription confirmed** notification.
      ![CreateAlarm](/images/temp/1/31.png?width=90pc)

5. Back to **Step 2: Configure actions** page.
    - Scroll down to the bottom and click the **Next** button.
      ![CreateAlarm](/images/temp/1/32.png?width=90pc)

6. At **Step 3: Add name and description** page.
    - Enter `fcj-fail-connect-to-dynamodb-alarm` at **Alarm name**.
    - Click the **Next** button.
      ![CreateAlarm](/images/temp/1/33.png?width=90pc)

7. At **Step 4: Preview and create** page.
    - Click the **Create alarm** button.
      ![CreateAlarm](/images/temp/1/34.png?width=90pc)

8. Open [AWS CloudWatch console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1).
    - Click the **All alarms** on the left menu.
    - You can see the **fcj-fail-connect-to-dynamodb-alarm** that we created in the previous step.
      ![CreateAlarm](/images/temp/1/35.png?width=90pc)

9. Open **Postman** to recall the api twice.
    ![CloudWatchMetrics](/images/temp/1/9.png?width=90pc)

10. Open the email that you chose to receive the notification before.
    - Find the email that is sent from <no-reply@sns.amazonaws.com>.
      ![CloudWatchMetrics](/images/temp/1/36.png?width=90pc)

11. Back to [AWS CloudWatch console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1).
    - Click the **All alarms** on the left menu.
    - Choose the **fcj-fail-connect-to-dynamodb-alarm** at **Name**.
      ![CloudWatchMetrics](/images/temp/1/37.png?width=90pc)
    - At **fcj-fail-connect-to-dynamodb-alarm** page, you can see the histogram of the metric is displayed.
      ![CloudWatchMetrics](/images/temp/1/38.png?width=90pc)

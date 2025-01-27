---
title : "Serverless - Monitoring Serverless app with CloudWatch and X-Ray"
date : "`r Sys.Date()`"
weight : 1
chapter : false
---
# Serverless - Monitoring Serverless app with CloudWatch and X-Ray

#### Overview
Application monitoring and observation is an important step in application deployment to ensure that all of the application's services are working properly and are capable of handling in the event of a failure. AWS provides a tool to help us do that like AWS CloudWatch, AWS X-Ray, and AWS CloudTrail. In this article, we will learn how to debug AWS Lambda through AWS CloudWatch, monitor Lambda using CloudWatch's built-in metrics or custom metrics that we define, and how trace the API using AWS X-Ray.

#### Amazon CloudWatch
Amazon CloudWatch monitors the AWS resources you use and the applications you run in real-time. Use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications. All the cloud services being used will provide metrics to CloudWatch and automatically display them when accessing the CloudWatch dashboard. CloudWatch also provides the following services:
- Metrics: a set of metrics integrated into the AWS services that are being used
- Logs: collect and store log files
- Events: send notifications in response to events
- Alarms: set trigger thresholds (alerts) to trigger an action

#### AWS X-ray
X-ray helps developers analyze and debug production, and distributed applications, such as those built using a microservices architecture. With X-ray, you can understand how your application and its underlying services work to identify and fix the root cause of performance problems and failures. X-ray provides an end-to-end view of requests as they move through your application and shows a map of the basic components of your application. You can use X-ray to analyze both development and production applications, from simple 3-level applications to complex microservices applications consisting of thousands of services.

#### Content

 1. [Preparation](1-preparation/)
 2. [Monitor with CloudWatch](2-build-sam-pipeline/)
 3. [Monitor with X-ray](3-build-frontend-pipeline/)
 4. [Resource Cleanup](4-cleanup)
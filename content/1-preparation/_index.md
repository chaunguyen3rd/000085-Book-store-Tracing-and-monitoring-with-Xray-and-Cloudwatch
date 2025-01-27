---
title : "Preparing"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---
Before doing the main content of this workshop, we prepare the services and data for the application.
1. Download the source code of the sam project below

{{%attachments title="SAM source" pattern=".*\.(zip)$"/%}}

2. Run the command below to deploy sam
```
sam build
sam deploy --guided
```
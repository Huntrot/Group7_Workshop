---
title : "API Integration"
date : "2025-01-15"
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

### Step 1: Get Your API Gateway URL
* First of all you have to receive API GateWay url 
```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev```
* Open your project folder in VS Code.
* Create a new file named api.js inside your src  directory.
* Add the following JavaScript code:
![apigateway_1](/images/5-Workshop/5.frontend/apigateway_1.png)
### Step 2: Create api.js in VS Code
* Create api.js
![apigateway_2](/images/5-Workshop/5.frontend/apigateway_2.png)
### Step 3: Configure Environment Variables
* Create a .env file in your project
* Add your API Gateway URL:
![apigateway_5](/images/5-Workshop/5.frontend/apigateway_5.png)
### Step 4: Test the API in Postman
* Take the data from postman
* You should receive a status 200 response with JSON data 
![apigateway_3](/images/5-Workshop/5.frontend/apigateway_3.png)
### Step 5: Deploy and Verify on S3
* Visit your **http://your-bucket-name.s3-website-ap-southeast-1.amazonaws.com**
* Using data from Postman with ```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev```
* If you login and it show a notification like this the API Integration was successfully
![apigateway_6](/images/5-Workshop/5.frontend/apigateway_6.png)
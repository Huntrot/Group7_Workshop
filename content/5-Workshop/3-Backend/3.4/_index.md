---
title : "Creating API Gateway and Integrating with Lambda"
date : 2025-09-09
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

### Objective
Connect **AWS API Gateway** with a **Lambda Function** to create a RESTful endpoint that allows access to data **DynamoDB**.


---


### Steps to Follow


#### **1. Access API Gateway**
- Go to **AWS Console → API Gateway**  
- Select **Create API**


![API\_1](/images/3.api-gateway/3.2/api_1.png)


- Choose type: **REST API (Build)**  


![API\_2](/images/3.api-gateway/3.2/api_2.png)


- Configure:
  - **Create new API:** New API
  - **API name:** `FlyoraAPI`
  - **Endpoint Type:** Regional
- Click **Create API**


![API\_3](/images/3.api-gateway/3.2/api_3.png)
---


#### **2. Tạo Resource và Method**
1. In the sidebar, select **Actions → Create Resource**


![API\_4](/images/3.api-gateway/3.2/api_4.png)


   - **Resource Name:** `api`
   - Click **Create Resource**


![API\_5](/images/3.api-gateway/3.2/api_10.png)


2. Select **/api → Actions → Create Resource**  


![API\_6](/images/3.api-gateway/3.2/api_12.png)


3. In the resource configuration:
   - Tick **Proxy resource**
   - **Resource path:** /api/  
   - **Resource Name:** `myProxy`
   - Click **Create resource**


![API\_7](/images/3.api-gateway/3.2/api_11.png)
---


#### **3. Attach Lambda**
1. After successfully creating **/api/{myProxy+}**, the **ANY** method appears:
   - Chọn **ANY → Integration request → Edit**
   - Then click **Create method**.


![API\_8](/images/3.api-gateway/3.2/api_13.png)

2. Attach Lambda:
   - **Integration type:** Lambda Function  
   - Tick **Lambda proxy integration**
   - **Lambda Region:** `ap-southeast-1` (Singapore)  
   - **Lambda Function:** select your `Lambda_API_Handler` function
---


#### **4. Deploy API**
- Select **Actions → Deploy API**
- **Deployment stage:** New stage  
  - **Stage name:** `dev`
  - **Description:** Development stage for Lambda API
- Click **Deploy**


![API\_9](/images/3.api-gateway/3.2/api_9.png)


After deployment, you will receive an Invoke URL like:
```https://<api_id>.execute-api.ap-southeast-1.amazonaws.com/dev```

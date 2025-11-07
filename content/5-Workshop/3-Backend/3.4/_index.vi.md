---
title : "Tạo API Gateway và tích hợp với Lambda"
date : "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

### Mục tiêu
Kết nối **AWS API Gateway** với **Lambda Function** để tạo endpoint RESTful cho phép truy cập dữ liệu trong **DynamoDB**.


---


### Các bước thực hiện


#### **1. Truy cập dịch vụ API Gateway**
- Vào **AWS Console → API Gateway**  
- Chọn **Create API**


![API\_1](/images/3.api-gateway/3.2/api_1.png)


- Chọn loại **REST API (Build)**  


![API\_2](/images/3.api-gateway/3.2/api_2.png)


- Chọn:
  - **Create new API:** New API
  - **API name:** `FlyoraAPI`
  - **Endpoint Type:** Regional
- Chọn **Create API**


![API\_3](/images/3.api-gateway/3.2/api_3.png)
---


#### **2. Tạo Resource và Method**
1. Trong sidebar, chọn **Actions → Create Resource**


![API\_4](/images/3.api-gateway/3.2/api_4.png)


   - **Resource Name:** `api`
   - Chọn **Create Resource**


![API\_5](/images/3.api-gateway/3.2/api_10.png)


2. Chọn **/api → Actions → Create Resource**  


![API\_6](/images/3.api-gateway/3.2/api_12.png)


3. Trong cấu hình resource:
   - Tick **Proxy resource**
   - **Resource path:** /api/  
   - **Resource Name:** `myProxy`
   - Nhấn **Create resource**


![API\_7](/images/3.api-gateway/3.2/api_11.png)
---


#### **3. Gắn Lambda**
1. Sau khi tạo thành công **/api/{myProxy+}**, xuất hiện method **ANY**:
   - Chọn **ANY → Integration request → Edit**
   - Tương tự rồi nhấn **Create method**.


![API\_8](/images/3.api-gateway/3.2/api_13.png)

2. Gắn Lambda:
   - **Integration type:** Lambda Function  
   - Tick **Lambda proxy integration**
   - **Lambda Region:** `ap-southeast-1` (Singapore)  
   - **Lambda Function:** chọn hàm `Lambda_API_Handler` của bạn
---


#### **4. Deploy API**
- Chọn **Actions → Deploy API**
- **Deployment stage:** New stage  
  - **Stage name:** `dev`
  - **Description:** Development stage for Lambda API
- Nhấn **Deploy**


![API\_9](/images/3.api-gateway/3.2/api_9.png)


Sau khi deploy, bạn sẽ nhận được Invoke URL dạng:
```https://<api_id>.execute-api.ap-southeast-1.amazonaws.com/dev```


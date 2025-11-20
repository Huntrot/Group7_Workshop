---
title : "Tạo API Gateway và tích hợp Lambda"
date : 2025-09-09
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---
### Mục tiêu
Tạo **Lambda Function** mới để xử lý các yêu cầu đến DynamoDB thông qua API Gateway.


## Các bước thực hiện

### Bước 1: Tạo IAM Role
1. Mở **IAM Console → Roles → Create role**  

![IAM\_1](/images/5-Workshop/3.api-gateway/3.1/iam_1.png)


![IAM\_2](/images/5-Workshop/3.api-gateway/3.1/iam_2.png)

1. Chọn **Trusted entity: AWS Service → Lambda**  

![IAM\_3](/images/3.api-gateway/3.1/iam_3.png)

2. Gán quyền:
   - `AmazonDynamoDBFullAccess`
   - `CloudWatchLogsFullAccess`
3. Đặt tên role: `LambdaAPIAccessRole`

![IAM\_4](/images/3.api-gateway/3.1/iam_4.png)
![IAM\_5](/images/3.api-gateway/3.1/iam_5.png)
---

### Bước 2: Tạo Lambda Function
1. Vào **AWS Lambda → Create function**
2. Chọn **Author from scratch**
3. Nhập tên: `DynamoDB_API_Handler`
4. Runtime: **Java 17**
5. Chọn IAM Role: `LambdaAPIAccessRole`

![LAMBDA\_1](/images/3.api-gateway/3.1/lambda_1.png)
---

### Bước 3: Deploy file jar 
        






![LAMBDA\_2](/images/3.api-gateway/3.1/lambda_3.png)

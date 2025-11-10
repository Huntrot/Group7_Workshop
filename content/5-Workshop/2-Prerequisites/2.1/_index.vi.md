---
title : "Tạo IAM User và phân quyền"
date : "2025-01-15"
weight : 1
chapter : false
pre : " <b> 5.2.1. </b> "
---

## Tạo IAM User và phân quyền
Trong bước này, bạn sẽ tạo một **IAM Role** cho Lambda và một **S3 Bucket** để lưu trữ file dữ liệu CSV trước khi import vào DynamoDB.

---

### Các bước thực hiện

#### 1. Truy cập dịch vụ IAM

* Vào **AWS Management Console** → tìm **IAM**.

![IAM\_1](/images/2.prerequisite/iam_1.png)

* Chọn **Roles** → **Create Role**.

![IAM\_2](/images/2.prerequisite/iam_2.png)

* Chọn **Trusted entity type:** *AWS service*.
* Chọn **Use case:** *Lambda*, sau đó nhấn **Next**.

![IAM\_3](/images/2.prerequisite/iam_3.png)

#### 2. Gán quyền truy cập cho Role

* Gắn các policy sau:

  * `AmazonS3FullAccess`
  * `AmazonDynamoDBFullAccess_v2`
* Nhấn **Next**, đặt tên role là `LambdaS3DynamoDBRole`.

![IAM\_4](/images/2.prerequisite/iam_4.png)
![IAM\_5](/images/2.prerequisite/iam_5.png)

{{% notice note %}}
Role này giúp Lambda có thể đọc file từ **S3** và ghi dữ liệu vào **DynamoDB**.
{{% /notice %}}

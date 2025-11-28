---
title : "Kiểm thử API bằng Postman"
date : "2025-01-15"
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---


### Mục tiêu
Kiểm thử API Gateway REST endpoint tích hợp với Lambda Function để xác nhận hoạt động dữ liệu DynamoDB.




---




{{% notice note %}}
Tải và cài đặt [Postman](https://dl.pstmn.io/download/latest/win64) trước khi bắt đầu phần này.
{{% /notice %}}


1. **Chỉnh lại Authorization**
- Vào **AWS Console → API Gateway**  
- Chọn **FlyoraAPI**
- Chọn **/api/v1/{myProxy+} → ANY → Method request → Edit**
![POST\_5](/images/3.api-gateway/3.3/post_5.png)
- Authorization: AWS_IAM
![POST\_6](/images/3.api-gateway/3.3/post_6.png)
1. **Tạo access key**
- Vào **AWS Console → IAM → Users**
- Nhấn **Create User**
![POST\_8](/images/3.api-gateway/3.3/post_8.png)
- Đặt tên: **test**
![POST\_7](/images/3.api-gateway/3.3/post_7.png)
- Xác nhận tạo iam user
![POST\_9](/images/3.api-gateway/3.3/post_9.png)
- Vào **test → Security credentiala → Create access key**
![POST\_10](/images/3.api-gateway/3.3/post_10.png)
- Chọn **Local code**
![POST\_11](/images/3.api-gateway/3.3/post_11.png)
- Copy access key và Secret access key
![POST\_12](/images/3.api-gateway/3.3/post_12.png)
### **Kiểm thử GET**
- Mở **Postman**
- Chọn **GET**
- Nhập URL:
```https://3b39kyvi8f.execute-api.ap-southeast-1.amazonaws.com/dev/api/v1/reviews/product/1```




- Tab **Headers**:
Key: `Content-Type` | Value: `application/json`
- Tab **Authorization**:
   - Type: AWS Signature
   - Nhap AccessKey
   - Nhap SecretKey
   - AWS Region: ap-southeast-1
   - Service Name: execute-api


- Nhấn **Send**  
- Kết quả: Trả về danh sách `Items` trong bảng **reviews**




![POST\_13](/images/3.api-gateway/3.3/post_13.png)
---




### **Kiểm thử POST**
- Chọn **POST**
- URL:
```https://3b39kyvi8f.execute-api.ap-southeast-1.amazonaws.com/dev/api/v1/reviews/submit```




- **Body → raw → JSON**
   ```json
   {
    "customerId": 2,
    "rating": 4,
    "comment": "Chim ăn ngon và vui vẻ!",
    "customerName": "Nguyễn Văn B"
  }
- Nhấn **Send**  
- Kết quả: Thêm `Items` trong bảng **Review**









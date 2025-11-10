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


### **Kiểm thử GET**
- Mở **Postman**
- Chọn **GET**
- Nhập URL:
```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev/api/account```


- Tab **Headers**:
Key: `Content-Type` | Value: `application/json`


- Nhấn **Send**  
- Kết quả: Trả về danh sách `Items` trong bảng **Account**


![POST\_1](/images/3.api-gateway/3.3/post_4.png)
---


### **Kiểm thử POST**
- Chọn **POST**
- URL:
```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev/api/account```


- **Body → raw → JSON**
   ```json
   {
  "item": {
    "id": "120",
    "username": "owner02",
    "password": "securepass",
    "email": "owner02@gmail.com",
    "phone": "0912345678",
    "role_id": "2",
    "is_active": "1",
    "is_approved": "1",
    "approved_by": "NULL"
  }
}
- Nhấn **Send**  
- Kết quả: Thêm `Items` trong bảng **Account**

![POST\_2](/images/3.api-gateway/3.3/post_3.png)

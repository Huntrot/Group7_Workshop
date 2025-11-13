---
title : "Kiểm tra dữ liệu trong DynamoDB"
date : "2025-01-15"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

## Kiểm tra dữ liệu trong DynamoDB
Trong bước này, bạn sẽ xác minh rằng dữ liệu từ file CSV đã được import thành công vào DynamoDB.

#### Các bước thực hiện

1. **Upload dữ liệu CSV lên Bucket**

{{% notice info %}}
Tải file mẫu CSV từ [đây](/files/database01.zip).
{{% /notice %}}

* Trong Bucket vừa tạo:

  * Chọn tab **Objects** → **Upload**.
  * Giải nén file zip.
  * Kéo & thả các **file**, sau đó nhấn **Upload**.

![test_1](/images/5-Workshop/2.prerequisite/test_1.png)
![test_2](/images/5-Workshop/2.prerequisite/test_2.png)
![test_3](/images/5-Workshop/2.prerequisite/test_3.png)

> [!TIP]
> Sau khi upload xong hãy đợi khoảng 3-5 phút để lambda import dữ liệu

1. **Truy cập dịch vụ DynamoDB**
   - Vào **AWS Management Console** → tìm **DynamoDB**.
   - Chọn **Tables** → click vào bảng `products`.

![test_4](/images/5-Workshop/2.prerequisite/test_4.png)

2. **Xem dữ liệu**
   - Chọn tab **Explore items**.

![test_5](/images/5-Workshop/2.prerequisite/test_5.png)

   - Kiểm tra danh sách sản phẩm đã được import.

![test_6](/images/5-Workshop/2.prerequisite/test_6.png)

{{% notice info %}}
Nếu không thấy dữ liệu, kiểm tra:
- Tên bảng DynamoDB phải trùng với tên file CSV.  
- File CSV phải có header hợp lệ.  
- Lambda có đủ quyền truy cập S3 và DynamoDB.
{{% /notice %}}

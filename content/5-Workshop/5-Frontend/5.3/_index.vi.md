---
title : "Kết nối với APU "
date : "2025-01-15"
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

###  Bước 1: Lấy URL của API Gateway
* Trước hết, bạn phải nhận được **URL của API Gateway**:
    ```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev
    ```
* Mở thư mục dự án của bạn trong **VS Code**.
* Tạo một tệp mới có tên là **api.js** bên trong thư mục `src`.
* Thêm mã JavaScript sau:
    ![apigateway_1](/images/5-Workshop/5.frontend/apigateway_1.png)



###  Bước 2: Tạo api.js trong VS Code
* Tạo tệp `api.js`:
    ![apigateway_2](/images/5-Workshop/5.frontend/apigateway_2.png)



###  Bước 3: Cấu hình Biến Môi trường
* Tạo một tệp **.env** trong dự án của bạn.
* Thêm **URL của API Gateway** của bạn:
    ![apigateway_5](/images/5-Workshop/5.frontend/apigateway_5.png)



###  Bước 4: Kiểm tra API trong Postman
* Lấy dữ liệu từ **Postman**.
* Bạn sẽ nhận được phản hồi với **mã trạng thái 200** kèm theo dữ liệu **JSON**:
    ![apigateway_3](/images/5-Workshop/5.frontend/apigateway_3.png)



###  Bước 5: Triển khai và Xác minh trên S3
* Truy cập trang web của bạn: **http://your-bucket-name.s3-website-ap-southeast-1.amazonaws.com**
* Sử dụng dữ liệu từ Postman với URL:
    ```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev
    ```
* Nếu bạn đăng nhập và thấy thông báo như thế này, thì việc **Tích hợp API** đã **thành công**:
    ![apigateway_6](/images/5-Workshop/5.frontend/apigateway_6.png)

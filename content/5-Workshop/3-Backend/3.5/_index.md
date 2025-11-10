---
title : "Test API using Postman"
date : "2025-01-15"
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

### Objective
Test the API Gateway REST endpoint integrated with a Lambda Function to verify DynamoDB data operations.


---


{{% notice note %}}
Download and install [Postman](https://dl.pstmn.io/download/latest/win64) before starting this section.
{{% /notice %}}


### **GET Test**
- Open **Postman**
- Select **GET**
- Enter URL:
```https://uwbxj9wfq6.execute-api.ap-southeast-1.amazonaws.com/dev/api/account```


- Tab **Headers**:
Key: `Content-Type` | Value: `application/json`


- Click **Send**  
- Result: Returns the list of `Items` in the table **Account**


![POST\_1](/images/3.api-gateway/3.3/post_4.png)
---


### **POST Test**
- Select **POST**
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
- Click **Send**  
- Result: Adds an `Items` in the table **Account**

![POST\_2](/images/3.api-gateway/3.3/post_3.png)


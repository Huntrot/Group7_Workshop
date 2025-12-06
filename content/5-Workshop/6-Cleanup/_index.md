---
title : "Clean Up Resources to Avoid AWS Charges"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Clean Up AWS Resources

To avoid unexpected charges, you need to delete the AWS resources created during this workshop in the following order:

---

### 1. Delete EventBridge Rule

1. Go to **AWS Console → EventBridge**
2. Select **Rules**
3. Select the **DatabaseBackup** rule
4. Click **Delete**
5. Confirm deletion

---

### 2. Delete API Gateway

1. Go to **AWS Console → API Gateway**
2. Select **FlyoraAPI**
3. Choose **Actions → Delete API**
4. Confirm deletion by entering the API name
5. Click **Delete**

---

### 3. Delete Lambda Functions

1. Go to **AWS Console → Lambda**
2. Delete the following Lambda functions:
   - **DynamoDB_API_Handler**
   - **AutoImportCSVtoDynamoDB**
   - **DatabaseBackupFunction**
3. For each function:
   - Select the function → **Actions → Delete**
   - Confirm deletion

---

### 4. Delete DynamoDB Tables

1. Go to **AWS Console → DynamoDB**
2. Select **Tables**
3. Delete all tables created from CSV files:
   - Select a table → **Delete**
   - Confirm deletion by typing `delete`
4. Repeat for all tables (Products, Orders, Customers, Reviews, etc.)

---

### 5. Delete S3 Buckets and Objects

#### 5.1. Delete Database Bucket

1. Go to **AWS Console → S3**
2. Select the **flyora-bucket-database** bucket
3. Delete all objects in the bucket:
   - Click **Empty bucket**
   - Confirm by typing `permanently delete`
4. After the bucket is empty:
   - Select the bucket → **Delete bucket**
   - Confirm by entering the bucket name

#### 5.2. Delete Backup Bucket

1. Select the **flyora-bucket-backup** bucket
2. Delete all objects:
   - Click **Empty bucket**
   - Confirm deletion
3. Delete the bucket:
   - Click **Delete bucket**
   - Confirm by entering the bucket name

---

### 6. Delete IAM User and Access Key

1. Go to **AWS Console → IAM → Users**
2. Select the **test** user
3. Go to the **Security credentials** tab
4. Delete the **Access Key** you created:
   - Select the Access Key → **Actions → Delete**
5. Return to the Users list
6. Select the **test** user → **Delete user**
7. Confirm deletion

---

### 7. Delete IAM Roles

#### 7.1. Delete LambdaAPIAccessRole

1. Go to **AWS Console → IAM → Roles**
2. Select **LambdaAPIAccessRole**
3. Detach the attached policies:
   - `AmazonDynamoDBFullAccess`
   - `CloudWatchLogsFullAccess`
   - `AWSXRayDaemonWriteAccess`
4. Click **Delete role**
5. Confirm deletion

#### 7.2. Delete LambdaS3DynamoDBRole

1. Select **LambdaS3DynamoDBRole**
2. Detach the policies:
   - `AmazonS3FullAccess`
   - `AmazonDynamoDBFullAccess_v2`
3. Click **Delete role**
4. Confirm deletion

#### 7.3. Delete LambdaDynamoDBBackupRole

1. Select **LambdaDynamoDBBackupRole**
2. Detach the policies:
   - `AmazonDynamoDBReadOnlyAccess`
   - `AmazonS3FullAccess`
   - `AWSLambdaBasicExecutionRole`
3. Click **Delete role**
4. Confirm deletion

---

### 8. Delete CloudWatch Logs

1. Go to **AWS Console → CloudWatch**
2. Select **Logs → Log groups**
3. Find and delete the related log groups:
   - `/aws/lambda/DynamoDB_API_Handler`
   - `/aws/lambda/AutoImportCSVtoDynamoDB`
   - `/aws/lambda/DatabaseBackupFunction`
   - `/aws/apigateway/FlyoraAPI`
4. Select log group → **Actions → Delete log group(s)**
5. Confirm deletion

---

### 9. Delete X-Ray Traces (Optional)

{{% notice info %}}
X-Ray traces automatically expire after 30 days and don't incur storage charges, but you can manually delete them if desired.
{{% /notice %}}

1. Go to **AWS Console → X-Ray**
2. Select **Traces**
3. Traces will be automatically deleted after the default retention period

---

### Final Verification

After completing the steps above, verify the following services to ensure no resources remain:

- ✅ **EventBridge**: No rules remaining
- ✅ **API Gateway**: No APIs remaining
- ✅ **Lambda**: No functions remaining (3 functions)
- ✅ **DynamoDB**: No tables remaining
- ✅ **S3**: No buckets remaining (2 buckets)
- ✅ **IAM Users**: No test user remaining
- ✅ **IAM Roles**: No created roles remaining (3 roles)
- ✅ **CloudWatch Logs**: No related log groups remaining
- ✅ **X-Ray**: Traces will expire automatically

{{% notice warning %}}
Make sure you've deleted all resources to avoid unexpected charges. Pay special attention to S3 buckets as they can accumulate data over time.
{{% /notice %}}

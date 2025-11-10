---
title : "Create IAM User & Configure Access Permissions"
date : "2025-01-15"
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

## Create IAM User & Access Setup
In this step, you will create an **IAM Role** for Lambda and an **S3 Bucket** to store CSV data files before importing them into DynamoDB.

---

### Steps to Perform

#### 1. Access the IAM Service

* Go to the **AWS Management Console** → search for **IAM**.

![IAM_1](/images/5-Workshop/2.prerequisite/iam_1.png)

* Select **Roles** → **Create Role**.

![IAM_2](/images/5-Workshop/2.prerequisite/iam_2.png)

* Choose **Trusted entity type:** *AWS service*.  
* Choose **Use case:** *Lambda*, then click **Next**.

![IAM_3](/images/5-Workshop/2.prerequisite/iam_3.png)

#### 2. Attach Permissions to the Role

* Attach the following policies:

  * `AmazonS3FullAccess`
  * `AmazonDynamoDBFullAccess_v2`

* Click **Next**, then name the role `LambdaS3DynamoDBRole`.

![IAM_4](/images/5-Workshop/2.prerequisite/iam_4.png)  
![IAM_5](/images/5-Workshop/2.prerequisite/iam_5.png)

{{% notice note %}}
This role allows Lambda to read files from **S3** and write data to **DynamoDB**.
{{% /notice %}}

---
title : "Workshop"
date :  "2025-01-15"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying the Flyora E-commerce System on AWS

### Overview

In this workshop, you will deploy the core components of the **Flyora** platform using a **Serverless** architecture on AWS.  
The objective is to build a system that is scalable, cost-efficient, and easy to maintain.

Components to be deployed:

- **Frontend**: Store & deliver UI via **S3 + CloudFront**
- **Backend API**: Handle business logic with **API Gateway + AWS Lambda**
- **Database**: Manage product / order data using **DynamoDB + S3**
- **User Authentication**: Implemented via **Amazon Cognito**
- **Chatbot**: Product consultation assistant integrated into UI (handled by AI Team)

The workshop is divided into group roles for parallel development:
**Backend (BE), AI (Chatbot), and Frontend (FE)**.

---

### System Architecture

![Flyora Architecture](/images/5-Workshop/flyora-architecture.png)

---

### Workshop Content

1. [Introduction: Objectives & Expected Outcomes](1-Introduction/)

2. [Environment Setup (Prerequisites)](2-Prerequisites/)
   - [Create IAM User + Configure Access Permissions](2-Prerequisites/2.1/)
   - [Something we will figure out later :))](2-Prerequisites/2.2/)

3. **Backend Workshop (BE)** — Build API + Automated Data Import Pipeline
   - [Prepare & upload CSV data to S3](3-Backend/3.1/)
   - [Create Lambda to automatically write CSV data to DynamoDB (S3 Trigger)](3-Backend/3.2/)
   - [Create API Gateway and integrate Lambda as Backend API](3-Backend/3.3/)
   - [Test API via Postman / API Gateway Console](3-Backend/3.4/)

4. **AI Workshop (Chatbot)** — Product Consultation Support
   - *(AI team will fill in content)* [](4-AI/4.1/)
   - *(AI team will fill in content)* [](4-AI/4.2/)
   - *(AI team will fill in content)* [](4-AI/4.3/)

5. **Frontend Workshop (FE)** — Display Data & Hosting website
   - [Hosting website with S3](5-Frontend/5.1/)
   -  [Distribute with CloudFront ](5-Frontend/5.2/)
   -  [API Integration](5-Frontend/5.3/)

6. [Set Up CI/CD for Automatic Deployment](6-CICD/)

7. [System Testing & Performance Evaluation](7-Testing/)

8. [Resource Cleanup to Avoid Unnecessary Charges](8-Cleanup/)

---

{{% notice info %}}
This workshop is designed to run within the **AWS Free Tier**,  
using no EC2, no SSH, and no paid services beyond free tier limits.
{{% /notice %}}

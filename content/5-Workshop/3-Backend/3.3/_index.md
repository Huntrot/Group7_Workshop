---
title : "Create API Gateway & Integrate with Lambda"
date : 2025-09-09
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Objective
Create a new **Lambda Function** to handle requests to DynamoDB via API Gateway..


## Steps to Follow

### Step 1: Create IAM Role
1. Open **IAM Console → Roles → Create role**  

![IAM_1](static/images/5-Workshop/3.api-gateway/3.1/iam_1.png)


![IAM\_2](/images/5-Workshop/3.api-gateway/3.1/iam_2.png)

1. Select **Trusted entity: AWS Service → Lambda**  

![IAM\_3](/images/3.api-gateway/3.1/iam_3.png)

2. Attach permissions:
   - `AmazonDynamoDBFullAccess`
   - `CloudWatchLogsFullAccess`
3. Name the role: `LambdaAPIAccessRole`

![IAM\_4](/images/3.api-gateway/3.1/iam_4.png)
![IAM\_5](/images/3.api-gateway/3.1/iam_5.png)
---

### Step 2: Create Lambda Function
1. Go to **AWS Lambda → Create function**
2. Select **Author from scratch**
3. Name: `DynamoDB_API_Handler`
4. Runtime: **Python 3.13**
5. Select IAM Role: `LambdaAPIAccessRole`

![LAMBDA\_1](/images/3.api-gateway/3.1/lambda_1.png)
---

### Bước 3: Paste Example Code
        ```python
        import boto3
        import json
        from decimal import Decimal

    dynamodb = boto3.resource('dynamodb')


    class DecimalEncoder(json.JSONEncoder):
        def default(self, obj):
            if isinstance(obj, Decimal):
                return int(obj) if obj % 1 == 0 else float(obj)
            return super(DecimalEncoder, self).default(obj)

    def lambda_handler(event, context):
        try:
            method = event.get('httpMethod', 'GET')
            path = event.get('path', '')  
            body = json.loads(event.get('body', '{}')) if event.get('body') else {}

            
            path_to_table = {
                '/api/account': 'Account',
                '/api/admin': 'Admin',
                '/api/role': 'Role',
                '/api/customer': 'Customer',
                '/api/staff': 'SalesStaff',
                '/api/owner': 'ShopOwner',
                '/api/product': 'Product',
                '/api/category': 'ProductCategory',
                '/api/review': 'ProductReview',
                '/api/promotion': 'Promotion',
                '/api/inventory': 'Inventory',
                '/api/type/food': 'FoodType',
                '/api/type/furniture': 'FurnitureType',
                '/api/type/toy': 'ToyType',
                '/api/type/bird': 'BirdType',
                '/api/detail/food': 'FoodDetail',
                '/api/detail/furniture': 'FurnitureDetail',
                '/api/detail/toy': 'ToyDetail',

        
                '/api/order': 'Order',
                '/api/order-item': 'OrderItem',
                '/api/payment': 'Payment',
                '/api/payment-method': 'PaymentMethod',
                '/api/delivery-note': 'DeliveryNote',
                '/api/shipping-method': 'ShippingMethod',

        
                '/api/notification': 'Notification',
                '/api/notification-type': 'NotificationType',
                '/api/chatbot': 'ChatBot',
                '/api/faq': 'FAQ',
                '/api/new': 'NewsArticle',

        
                '/api/log/access': 'AccessLog',
                '/api/log/system': 'SystemLog',
                '/api/issue': 'IssueReport',
                '/api/policy': 'Policy'
    }
            

            
            table_name = path_to_table.get(path)
            if not table_name:
                return {
                    'statusCode': 404,
                    'body': json.dumps({'error': f'Unknown API path: {path}'})
                }

            table = dynamodb.Table(table_name)

            
            if method == 'GET':
                response = table.scan()
                return {
                    'statusCode': 200,
                    'headers': {'Content-Type': 'application/json'},
                    'body': json.dumps(response['Items'], cls=DecimalEncoder)
                }

            
            elif method == 'POST':
                item = body.get('item')
                if not item:
                    return {
                        'statusCode': 400,
                        'body': json.dumps({'error': 'Missing item data'})
                    }
                table.put_item(Item=item)
                return {
                    'statusCode': 200,
                    'headers': {'Content-Type': 'application/json'},
                    'body': json.dumps({'message': 'Item added successfully', 'item': item}, cls=DecimalEncoder)
                }

            
            elif method == 'PUT':
                item = body.get('item')
                if not item:
                    return {
                        'statusCode': 400,
                        'body': json.dumps({'error': 'Missing item data for update'})
                    }
                table.put_item(Item=item)
                return {
                    'statusCode': 200,
                    'headers': {'Content-Type': 'application/json'},
                    'body': json.dumps({'message': 'Item updated successfully', 'item': item}, cls=DecimalEncoder)
                }

            
            elif method == 'DELETE':
                key = body.get('key')
                if not key:
                    return {
                        'statusCode': 400,
                        'body': json.dumps({'error': 'Missing key for delete'})
                    }
                table.delete_item(Key=key)
                return {
                    'statusCode': 200,
                    'body': json.dumps({'message': 'Item deleted successfully'})
                }

            
            else:
                return {
                    'statusCode': 405,
                    'body': json.dumps({'error': f'Method {method} not allowed'})
                }

        except Exception as e:
            return {
                'statusCode': 500,
                'body': json.dumps({'error': str(e)})
            }






![LAMBDA\_2](/images/3.api-gateway/3.1/lambda_3.png)

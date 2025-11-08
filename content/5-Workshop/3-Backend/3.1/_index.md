---
title : "Prepare & Configure Lambda Trigger for S3"
date : 2025-09-09
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

## Prepare & Configure Lambda Trigger for S3
#### Create an S3 Bucket

* Go to the **S3** service.

![S3_1](/images/5-Workshop/2.prerequisite/S3_1.png)

* In the **S3** interface, select **Create bucket**.

![S3_2](/images/5-Workshop/2.prerequisite/S3_2.png)

* On the **Create bucket** screen:

  * **Bucket name:** Enter a name, for example:

    ```text
    flyora-bucket
    ```
  * (If the name already exists, add a number at the end.)

{{% notice note %}}
Keep all other default settings unchanged.
{{% /notice %}}

![S3_3](/images/5-Workshop/2.prerequisite/S3_3.png)

* Review your configuration and click **Create bucket** to finish.

---

### Expected Results

- The **`flyora-bucket`** (or your chosen name) is successfully created.  
- The **`LambdaS3DynamoDBRole`** role is ready to be assigned to Lambda in the next step.

#### Configure Lambda Trigger for S3

In this step, you will configure **AWS Lambda** to automatically import CSV files into **DynamoDB** whenever a new file is uploaded to the **S3 Bucket**.

1. **Create a Lambda Function**
   - Go to **Lambda** → **Create function**.

![lambda_1](/images/5-Workshop/2.prerequisite/lambda_1.png)

   - Select **Author from scratch**.
   - Function name: `AutoImportCSVtoDynamoDB`.
   - Runtime: **Python 3.13**.
   - Role: select **LambdaS3DynamoDBRole** created in the previous step.

![lambda_2](/images/5-Workshop/2.prerequisite/lambda_2.png)

2. **Add a Trigger**
   - In the **Configuration → Triggers** tab, click **Add trigger**.

![lambda_3](/images/5-Workshop/2.prerequisite/lambda_3.png)

   - Choose **S3**.
   - Select Bucket `flyora-bucket`.
   - Event type: `All object create events`.

![lambda_4](/images/5-Workshop/2.prerequisite/lambda_4.png)

   - Click **Add** to save.

![lambda_5](/images/5-Workshop/2.prerequisite/lambda_5.png)

3. **Paste the Lambda Code**
   - Paste the following code:

    ```python
    import boto3
    import csv
    import io
    import json
    from botocore.exceptions import ClientError

    dynamodb = boto3.resource('dynamodb')
    s3 = boto3.client('s3')

    def create_table_if_not_exists(table_name, sample_item):
        existing_tables = dynamodb.meta.client.list_tables()['TableNames']
        if table_name in existing_tables:
            print(f"Table '{table_name}' already exists.")
            return

        # Use the first key as the Partition key
        partition_key = list(sample_item.keys())[0]

        print(f"Creating new table: {table_name} with partition key: {partition_key}")
        table = dynamodb.create_table(
            TableName=table_name,
            KeySchema=[{'AttributeName': partition_key, 'KeyType': 'HASH'}],
            AttributeDefinitions=[{'AttributeName': partition_key, 'AttributeType': 'S'}],
            BillingMode='PAY_PER_REQUEST'
        )
        table.wait_until_exists()
        print(f"Table '{table_name}' created successfully.")

    def lambda_handler(event, context):
        try:
            for record in event['Records']:
                bucket = record['s3']['bucket']['name']
                key = record['s3']['object']['key']

                print(f"Processing file: {key} from bucket: {bucket}")

                response = s3.get_object(Bucket=bucket, Key=key)
                body = response['Body'].read().decode('utf-8')

                reader = csv.DictReader(io.StringIO(body))
                items = list(reader)
                if not items:
                    print(f"File {key} is empty, skipping.")
                    continue

                table_name = key.split('.')[0]  # table name = file name (without .csv)
                create_table_if_not_exists(table_name, items[0])

                table = dynamodb.Table(table_name)
                with table.batch_writer() as batch:
                    for item in items:
                        batch.put_item(Item=item)
                print(f"Imported {len(items)} records into {table_name}")

            return {
                'statusCode': 200,
                'body': json.dumps('Import completed successfully')
            }

        except ClientError as e:
            print(f"AWS Error: {e.response['Error']['Message']}")
            return {'statusCode': 500, 'body': json.dumps('AWS Error')}
        except Exception as e:
            print(f"General Error: {str(e)}")
            return {'statusCode': 500, 'body': json.dumps('General Error')}
    ```

![lambda_6](/images/5-Workshop/2.prerequisite/lambda_6.png)

   - Click **Deploy** and confirm it shows **Successfully**.

![lambda_7](/images/5-Workshop/2.prerequisite/lambda_7.png)

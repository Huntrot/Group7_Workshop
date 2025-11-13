---
title : "Chuẩn bị & Cấu hình Lambda Trigger cho S3"
date : "2025-01-15"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

## Chuẩn bị & Cấu hình Lambda Trigger cho S3
#### Tạo S3 Bucket

* Truy cập dịch vụ **S3**.

![S3\_1](/images/5-Workshop/2.prerequisite/S3_1.png)

* Trong giao diện **S3**, chọn **Create bucket**.

![S3\_2](/images/5-Workshop/2.prerequisite/S3_2.png)

* Trong màn hình **Create bucket**:

  * **Bucket name:** Nhập tên, ví dụ:

    ```text
    flyora-bucket-database
    ```
  * (Nếu tên đã tồn tại, hãy thêm số phía sau.)

{{% notice note %}}
Giữ nguyên các thiết lập mặc định còn lại.
{{% /notice %}}

![S3\_2](/images/5-Workshop/2.prerequisite/S3_3.png)

* Xem lại cấu hình và chọn **Create bucket** để hoàn tất.

---

### Kết quả mong đợi

- Bucket **`flyora-bucket`** (hoặc tên bạn đã đặt) được tạo thành công.
- Role **`LambdaS3DynamoDBRole`** sẵn sàng để gán cho Lambda trong bước kế tiếp.

#### Cấu hình Lambda Trigger cho S3

Trong bước này, bạn sẽ cấu hình **AWS Lambda** để tự động import file CSV vào **DynamoDB** mỗi khi có file mới trong **S3 Bucket**.


1. **Tạo Lambda Function**
   - Truy cập **Lambda** → **Create function**.

![lambda_1](/images/5-Workshop/2.prerequisite/lambda_1.png)

   - Chọn **Author from scratch**.
   - Đặt tên: `AutoImportCSVtoDynamoDB`.
   - Runtime: **Python 3.13**.
   - Role: chọn **LambdaS3DynamoDBRole** đã tạo ở bước trước.

![lambda_2](/images/5-Workshop/2.prerequisite/lambda_2.png)

2. **Thêm Trigger**
   - Trong tab **Configuration → Triggers**, nhấn **Add trigger**.

![lambda_3](/images/5-Workshop/2.prerequisite/lambda_3.png)

   - Chọn **S3**.
   - Chọn Bucket `flyora-bucket`.
   - Event type: `All object create events`.

![lambda_4](/images/5-Workshop/2.prerequisite/lambda_4.png)

   - Nhấn **Add** để lưu.

![lambda_5](/images/5-Workshop/2.prerequisite/lambda_5.png)

1. **Dán code Lambda**
   - Dán đoạn code dưới đây:
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

        # Lấy key đầu tiên làm Partition key
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

                table_name = key.split('.')[0]  # tên bảng = tên file (không có .csv)
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



![lambda_6](/images/5-Workshop/2.prerequisite/lambda_6.png)

  - Nhấn **Deploy** và xác nhận **Successfully**.

![lambda_7](/images/5-Workshop/2.prerequisite/lambda_7.png)
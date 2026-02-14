📸 Serverless Image Processing Pipeline (AWS)
📌 Project Overview

This project demonstrates a serverless event-driven architecture using AWS services.

When a user uploads an image into the input/ folder of an S3 bucket:

Amazon S3 triggers AWS Lambda

Lambda processes the file (copy or resize logic)

The processed file is stored in the output/ folder

Logs are stored in CloudWatch

The infrastructure is provisioned using CloudFormation (Infrastructure as Code).

🏗️ Architecture

<img width="1536" height="1024" alt="ChatGPT Image Feb 15, 2026, 01_42_18 AM" src="https://github.com/user-attachments/assets/7fa2ce41-9325-4d85-a61e-12f663540984" />




Flow:

User uploads image to input/ in S3

S3 triggers Lambda function

Lambda processes the image

Processed image is saved to output/

Logs are stored in CloudWatch

🧩 AWS Services Used

Amazon S3 – Object storage

AWS Lambda – Serverless compute

IAM – Permissions for Lambda

CloudWatch – Logs & monitoring

CloudFormation – Infrastructure as Code

📂 S3 Structure
image-processing-bucket/
   ├── input/
   └── output/


Upload images inside input/

Processed images appear inside output/

⚙️ Implementation Steps (Simple Version)
1️⃣ Create Infrastructure

Deploy CloudFormation template to create:

S3 bucket

Lambda function

IAM role

Lambda permissions

2️⃣ Configure S3 Trigger

In S3 bucket:

Go to Properties → Event Notifications

Create event:

Event type: All object create events

Prefix: input/

Destination: Lambda function

3️⃣ Lambda Logic (Simple Copy Version)
import boto3

s3 = boto3.client('s3')

def lambda_handler(event, context):

    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']

    if not key.startswith("input/"):
        return

    output_key = key.replace("input/", "output/")

    s3.copy_object(
        Bucket=bucket,
        CopySource={'Bucket': bucket, 'Key': key},
        Key=output_key
    )

    print("File copied successfully")

🧪 How to Test

Upload any image inside:

input/


Wait a few seconds

Check:

output/


The image will be automatically copied.

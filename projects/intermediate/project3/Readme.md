# Automated AWS Receipt Processing System ☁️

## **1. Overview**
This project automates receipt processing using **AWS serverless services**. It eliminates manual data entry and provides a scalable, cost-effective way to handle receipts by extracting, storing, and notifying users in real-time.

**Key Features**:
- Store receipt images and PDFs in **Amazon S3**  
- Extract structured text using **Amazon Textract**  
- Save processed data in **DynamoDB**  
- Send email notifications via **Amazon SES**  
- Trigger everything automatically with **AWS Lambda**

---

## **2. Architecture Diagram**
<img width="1415" height="667" alt="f3b95d6339d8f8f3b7d93b8a849f8480" src="https://github.com/user-attachments/assets/9d92892d-35c5-4afb-8608-e5e94853ff57" />

---

## **3. Services Used**
| **Service** | **Purpose** |
|-------------|-------------|
| Amazon S3 | Stores receipt images and PDFs |
| Amazon Textract | Extracts structured text and fields |
| Amazon DynamoDB | Stores structured receipt data |
| Amazon SES | Sends email notifications |
| AWS Lambda | Orchestrates workflow automation |
| IAM Roles/Policies | Provides secure permissions |

---

## **4. Project Setup**

### **4.1 Storage and Database Setup**
#### **S3 Buckets**
- Create **3 buckets**: `raw`, `processed`, and `final`.
- Add an **incoming folder** in the raw bucket for uploads.
![S3 Buckets](images/s3-bucket-setup.png)

#### **DynamoDB Table**
- Table name: `Receipts`
- Partition Key: `receipt_id` (String)
- Sort Key: `date` (String)
<img width="1095" height="580" alt="Screenshot 2025-08-24 at 5 58 16 PM" src="https://github.com/user-attachments/assets/9130ebac-d4e6-4f9a-aadc-4eb2f237aa12" />


---

### **4.2 Notification Setup (Amazon SES)**
1. Navigate to SES → **Identities**  
2. Verify sender and recipient emails  
3. Confirm successful verification  
![SES Setup](images/ses-setup.png)

---

### **4.3 Processing Setup (Lambda)**
#### **IAM Role**
Attach:
- `AmazonS3ReadOnlyAccess`
- `AmazonTextractFullAccess`
- `AmazonDynamoDBFullAccess`
- `AmazonSESFullAccess`
- `AWSLambdaBasicExecutionRole`

![IAM Role](images/iam-role.png)

#### **Lambda Function**
- Runtime: Python 3.9
- Timeout: 180s
- Add environment variables:
  - `DYNAMODB_TABLE`
  - `SES_SENDER_EMAIL`
  - `SES_RECIPIENT_EMAIL`

Deploy the provided script:  
[lambda/receipt_processor.py](lambda/receipt_processor.py)

---

### **4.4 Integration and Testing**
#### **Integration**
- Add **S3 event notification** to trigger Lambda.
![S3 Event](images/s3-event.png)

#### **Testing**
- Upload a test receipt to S3.
- Check CloudWatch logs:
![CloudWatch Logs](images/cloudwatch-logs.png)
- Verify DynamoDB entry:
![DynamoDB Items](images/dynamodb-items.png)
- Check email notification:
![SES Email](images/email-notification.png)

---

## **5. Project Improvements**
- Add a simple upload form hosted on S3
- Categorize receipts in DynamoDB
- Generate monthly summary reports
- Optimize images to save storage costs
- Set up SNS for error alerts
- Add authentication with Cognito

---

## **6. Lessons Learned**
- Importance of **IAM permissions** and least privilege  
- CloudWatch logs are critical for **debugging Lambda issues**  
- Textract handles most receipts well but benefits from **pre-processing for clarity**

---

## **7. Resources**
- [AWS Textract Documentation](https://docs.aws.amazon.com/textract/)  
- [AWS Lambda Guide](https://docs.aws.amazon.com/lambda/)  
- [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/)

---

## **8. License**
This project is released under the [MIT License](LICENSE).

---


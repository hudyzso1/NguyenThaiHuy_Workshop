---
title: "Testing and Cleanup"
date: 2026-07-14
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

# System Testing and Resource Cleanup

In this section, we will test the main components of the **CyberNet** system after deployment on AWS.

The components to test include the Frontend on Amazon S3 and CloudFront, the Backend using API Gateway and Lambda, PayOS payment integration, data stored on Amazon S3, and the VPC network infrastructure.

After testing is completed, unused AWS resources should be deleted to avoid unexpected costs.

---

## 1. Test the Frontend

Access the CyberNet website through the CloudFront domain.

Example:

```text
https://xxxxxxxxxxxx.cloudfront.net
```

Check the following items:

- The website is displayed successfully.
- CSS, JavaScript, and images are loaded correctly.
- Main pages work normally.
- Product data and images are displayed correctly.
- No browser errors occur during access.

---

## 2. Test API Gateway and Lambda

Copy the Invoke URL from API Gateway to test the created endpoint.

Example:

```text
https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/payment/create
```

Send a sample request:

```json
{
  "amount": 50000
}
```

Expected result:

```json
{
  "message": "Payment request created successfully"
}
```

Check the following items:

- API Gateway receives the request successfully.
- Lambda Function processes the request correctly.
- The response is returned in JSON format.
- No errors occur during processing.

---

## 3. Test PayOS Payment

Test the payment creation function through the Backend.

Testing flow:

1. The Frontend sends a payment request.
2. API Gateway receives the request.
3. Lambda processes the payment data.
4. Lambda creates payment information with PayOS.
5. The Frontend receives the response.

Expected result:

- The payment request is created successfully.
- The system returns payment information.
- The user can continue the payment process.
- No errors occur during processing.

---

## 4. Test Data on Amazon S3

Check the files and resources uploaded to Amazon S3.

Items to check:

- Frontend files are uploaded completely.
- Product images are stored in the correct folder.
- Image paths work correctly.
- The Frontend can load images from S3 or CloudFront.
- No sensitive files are uploaded to S3.

Example folders:

```text
products/
images/
uploads/
```

---

## 5. Check CloudWatch Logs

Open the **Amazon CloudWatch Console** to check the logs of the Lambda Function.

Items to check:

- Requests sent to Lambda.
- Responses returned by Lambda.
- Errors during processing.
- Lambda execution time.

CloudWatch Logs help monitor Backend activity and support troubleshooting when issues occur.

---

## 6. Clean Up AWS Resources

After completing the Workshop, delete unused AWS resources.

Resources to clean up include:

- CloudFront Distribution.
- S3 Bucket and objects inside the bucket.
- API Gateway.
- Lambda Function.
- CloudWatch Log Group.
- IAM Role used by Lambda.
- VPC, Subnet, Route Table, and Internet Gateway if they are no longer used.

---

## 7. Check Cost After Cleanup

After deleting resources, open **AWS Billing and Cost Management** to check costs.

Make sure that:

- No unexpected resources are still running.
- No unused CloudFront Distribution remains.
- No unnecessary S3 Bucket remains.
- No Lambda Function or API Gateway from the Workshop remains active.
- AWS costs are controlled after finishing the practice.

---

## Result

After completing this section:

- The CyberNet Frontend has been tested.
- API Gateway and Lambda work correctly.
- PayOS payment functionality has been tested.
- Data on Amazon S3 has been verified.
- CloudWatch Logs are used for monitoring.
- Unused AWS resources have been cleaned up.
- AWS costs are controlled after completing the Workshop.
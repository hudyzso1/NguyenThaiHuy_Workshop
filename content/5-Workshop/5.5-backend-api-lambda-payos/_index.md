---
title: "Backend API and PayOS"
date: 2026-07-14
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

In this section, we will build the backend for the **CyberNet** system using **Amazon API Gateway**, **AWS Lambda**, and **PayOS** payment integration.

API Gateway is used to receive requests from the frontend. AWS Lambda is used to process backend logic such as creating orders, handling payment requests, and returning data to the frontend. PayOS is used to support the payment feature in the system.

---

## 1. Create Lambda Function

Open the **AWS Lambda Console** and choose **Create function**.

Configure the Lambda Function as follows:

| Parameter | Value |
| :--- | :--- |
| **Function name** | `cybernet-payment-lambda` |
| **Runtime** | Node.js |
| **Architecture** | x86_64 |
| **Execution role** | Create a new role with basic Lambda permissions |

<img src="/images/5-Workshop/5.5-backend-api-lambda-payos/1.png" alt="Create Lambda Function for CyberNet" width="800">

---

## 2. Configure Environment Variables

In the Lambda Function, open the **Configuration** tab and choose **Environment variables**.

Add the required environment variables for PayOS:

| Key | Value |
| :--- | :--- |
| `PAYOS_CLIENT_ID` | PayOS Client ID |
| `PAYOS_API_KEY` | PayOS API Key |
| `PAYOS_CHECKSUM_KEY` | PayOS Checksum Key |
| `PAYOS_RETURN_URL` | Return URL after successful payment |
| `PAYOS_CANCEL_URL` | Cancel URL when payment is cancelled |

---

## 3. Add Payment Handling Logic

In the Lambda code editor, add the logic to handle payment creation requests.

Example:

```javascript
export const handler = async (event) => {
  const body = JSON.parse(event.body || "{}");

  const order = {
    orderCode: Date.now(),
    amount: body.amount,
    description: "CyberNet payment",
    returnUrl: process.env.PAYOS_RETURN_URL,
    cancelUrl: process.env.PAYOS_CANCEL_URL
  };

  return {
    statusCode: 200,
    headers: {
      "Content-Type": "application/json",
      "Access-Control-Allow-Origin": "*"
    },
    body: JSON.stringify({
      message: "Payment request created successfully",
      data: order
    })
  };
};
```

---

## 4. Create API Gateway

Open the **Amazon API Gateway Console** and choose **Create API**.

Select:

```text
HTTP API
```

Configure the API as follows:

| Parameter | Value |
| :--- | :--- |
| **API name** | `cybernet-api` |
| **Integration** | Lambda |
| **Lambda function** | `cybernet-payment-lambda` |
| **Region** | `ap-southeast-1` |

---

## 5. Create API Route

Create a route to send payment requests from the frontend to Lambda.

| Method | Route |
| :--- | :--- |
| `POST` | `/payment/create` |

This route is connected to the `cybernet-payment-lambda` function.

---

## 6. Test the API

After creating API Gateway, copy the **Invoke URL** to test the API.

Example:

```text
https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/payment/create
```

Sample request:

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

---

## Result

After completing this section, the CyberNet backend has:

- A Lambda Function for payment processing logic.
- API Gateway to receive requests from the frontend.
- An API route for payment creation.
- Environment variables for PayOS integration.
- A backend foundation ready to connect with the frontend.
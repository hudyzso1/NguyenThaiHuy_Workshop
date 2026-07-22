---
title: "Backend API và PayOS"
date: 2026-07-14
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

Trong phần này, chúng ta sẽ xây dựng Backend cho hệ thống **CyberNet** bằng **Amazon API Gateway**, **AWS Lambda** và tích hợp thanh toán với **PayOS**.

API Gateway đóng vai trò tiếp nhận request từ Frontend. AWS Lambda xử lý logic nghiệp vụ như tạo đơn hàng, xử lý yêu cầu thanh toán và trả dữ liệu về cho Frontend. PayOS được sử dụng để hỗ trợ chức năng thanh toán trong hệ thống.

---

## 1. Tạo Lambda Function

Mở **AWS Lambda Console** và chọn **Create function**.

Cấu hình Lambda Function như sau:

| Thông số | Giá trị |
| :--- | :--- |
| **Function name** | `cybernet-payment-lambda` |
| **Runtime** | Node.js |
| **Architecture** | x86_64 |
| **Execution role** | Create a new role with basic Lambda permissions |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.5-backend-api-lambda-payos/1.png" alt="Tạo Lambda Function cho CyberNet" width="800">

---

## 2. Cấu hình biến môi trường cho Lambda

Trong Lambda Function, mở tab **Configuration** và chọn **Environment variables**.

Thêm các biến môi trường cần thiết cho PayOS:

| Key | Value |
| :--- | :--- |
| `PAYOS_CLIENT_ID` | Client ID của PayOS |
| `PAYOS_API_KEY` | API Key của PayOS |
| `PAYOS_CHECKSUM_KEY` | Checksum Key của PayOS |
| `PAYOS_RETURN_URL` | URL trả về sau khi thanh toán |
| `PAYOS_CANCEL_URL` | URL khi hủy thanh toán |

---

## 3. Viết logic xử lý thanh toán

Trong phần code của Lambda, thêm logic xử lý request tạo thanh toán.

Ví dụ:

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

## 4. Tạo API Gateway

Mở **Amazon API Gateway Console** và chọn **Create API**.

Chọn loại API:

```text
HTTP API
```

Cấu hình API như sau:

| Thông số | Giá trị |
| :--- | :--- |
| **API name** | `cybernet-api` |
| **Integration** | Lambda |
| **Lambda function** | `cybernet-payment-lambda` |
| **Region** | `ap-southeast-1` |

---
<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.5-backend-api-lambda-payos/1.png" alt="API Gate Way" width="800">


## 5. Tạo route cho API

Tạo route dùng để gửi request thanh toán từ Frontend đến Lambda.

| Method | Route |
| :--- | :--- |
| `POST` | `/payment/create` |

Route này sẽ được kết nối với Lambda Function `cybernet-payment-lambda`.

---

## 6. Kiểm tra API

Sau khi tạo API Gateway, sao chép **Invoke URL** để kiểm tra API.

Ví dụ:

```text
https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/payment/create
```

Gửi request mẫu:

```json
{
  "amount": 50000
}
```

Kết quả mong đợi:

```json
{
  "message": "Payment request created successfully"
}
```

---

## Kết quả

Sau khi hoàn thành phần này, Backend của CyberNet đã có:

- Lambda Function dùng để xử lý logic thanh toán.
- API Gateway dùng để nhận request từ Frontend.
- Route API cho chức năng tạo thanh toán.
- Biến môi trường phục vụ tích hợp PayOS.
- Backend sẵn sàng kết nối với giao diện Frontend.
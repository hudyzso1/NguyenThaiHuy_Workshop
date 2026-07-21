---
title: "Kiểm thử và dọn dẹp"
date: 2026-07-14
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

# Kiểm thử hệ thống và dọn dẹp tài nguyên

Trong phần này, chúng ta sẽ kiểm thử các thành phần chính của hệ thống **CyberNet** sau khi triển khai trên AWS.

Các thành phần cần kiểm thử gồm Frontend trên Amazon S3 và CloudFront, Backend sử dụng API Gateway và Lambda, chức năng thanh toán PayOS, dữ liệu lưu trên Amazon S3 và hạ tầng mạng VPC.

Sau khi kiểm thử hoàn tất, cần dọn dẹp các tài nguyên không còn sử dụng để tránh phát sinh chi phí.

---

## 1. Kiểm thử Frontend

Truy cập website CyberNet thông qua CloudFront domain.

Ví dụ:

```text
https://xxxxxxxxxxxx.cloudfront.net
```

Kiểm tra các nội dung sau:

- Website hiển thị thành công.
- Giao diện tải đúng CSS, JavaScript và hình ảnh.
- Các trang chính hoạt động bình thường.
- Dữ liệu sản phẩm và hình ảnh được hiển thị đúng.
- Không xảy ra lỗi khi truy cập bằng trình duyệt.

---

## 2. Kiểm thử API Gateway và Lambda

Sao chép Invoke URL của API Gateway để kiểm tra endpoint đã tạo.

Ví dụ:

```text
https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/payment/create
```

Gửi request kiểm thử:

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

Cần kiểm tra:

- API Gateway nhận request thành công.
- Lambda Function xử lý request đúng.
- Response trả về đúng định dạng JSON.
- Không có lỗi trong quá trình xử lý.

---

## 3. Kiểm thử thanh toán PayOS

Kiểm tra chức năng tạo yêu cầu thanh toán thông qua Backend.

Quy trình kiểm thử:

1. Frontend gửi request tạo thanh toán.
2. API Gateway nhận request.
3. Lambda xử lý dữ liệu thanh toán.
4. Lambda tạo thông tin thanh toán với PayOS.
5. Frontend nhận kết quả trả về.

Kết quả mong đợi:

- Request thanh toán được tạo thành công.
- Hệ thống trả về thông tin thanh toán.
- Người dùng có thể tiếp tục quy trình thanh toán.
- Không xảy ra lỗi trong quá trình xử lý.

---

## 4. Kiểm thử dữ liệu trên Amazon S3

Kiểm tra các file và tài nguyên đã upload lên Amazon S3.

Các nội dung cần kiểm tra:

- File Frontend đã được upload đầy đủ.
- Hình ảnh sản phẩm được lưu đúng thư mục.
- Đường dẫn hình ảnh hoạt động đúng.
- Frontend có thể tải hình ảnh từ S3 hoặc CloudFront.
- Không có file nhạy cảm được upload lên S3.

Ví dụ thư mục lưu trữ:

```text
products/
images/
uploads/
```

---

## 5. Kiểm tra CloudWatch Logs

Mở **Amazon CloudWatch Console** để kiểm tra log của Lambda Function.

Các nội dung cần kiểm tra:

- Request gửi vào Lambda.
- Response trả về từ Lambda.
- Lỗi phát sinh trong quá trình xử lý.
- Thời gian thực thi của Lambda.

CloudWatch Logs giúp theo dõi hoạt động của Backend và hỗ trợ xử lý lỗi khi hệ thống gặp sự cố.

---

## 6. Dọn dẹp tài nguyên AWS

Sau khi hoàn thành Workshop, cần xóa các tài nguyên không còn sử dụng.

Các tài nguyên cần dọn dẹp gồm:

- CloudFront Distribution.
- S3 Bucket và các object bên trong.
- API Gateway.
- Lambda Function.
- CloudWatch Log Group.
- IAM Role dùng cho Lambda.
- VPC, Subnet, Route Table và Internet Gateway nếu không còn sử dụng.

---

## 7. Kiểm tra chi phí sau khi dọn dẹp

Sau khi xóa tài nguyên, mở **AWS Billing and Cost Management** để kiểm tra chi phí.

Cần đảm bảo:

- Không còn tài nguyên đang chạy ngoài ý muốn.
- Không còn CloudFront Distribution không sử dụng.
- Không còn S3 Bucket chứa dữ liệu không cần thiết.
- Không còn Lambda Function hoặc API Gateway phục vụ Workshop.
- Chi phí AWS được kiểm soát sau khi kết thúc bài thực hành.

---

## Kết quả

Sau khi hoàn thành phần này:

- Frontend CyberNet đã được kiểm thử.
- API Gateway và Lambda hoạt động đúng.
- Chức năng thanh toán PayOS được kiểm tra.
- Dữ liệu trên Amazon S3 được xác nhận.
- CloudWatch Logs được sử dụng để theo dõi lỗi.
- Các tài nguyên AWS không còn sử dụng đã được dọn dẹp.
- Chi phí AWS được kiểm soát sau khi hoàn thành Workshop.
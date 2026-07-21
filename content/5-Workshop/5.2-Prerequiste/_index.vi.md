---
title: "Các bước chuẩn bị"
date: 2026-07-14
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---



Trước khi triển khai hệ thống **CyberNet quản lý phòng máy** trên AWS, cần chuẩn bị tài khoản, quyền truy cập, công cụ phát triển và các thông số cần thiết.

CyberNet sử dụng các dịch vụ AWS chính sau:

- Amazon S3
- Amazon CloudFront
- Amazon API Gateway
- AWS Lambda
- Amazon CloudWatch
- AWS IAM
- PayOS API

---

## 1. Chuẩn bị tài khoản AWS

Trước tiên, cần chuẩn bị một tài khoản AWS có thể tạo và quản lý tài nguyên Cloud.

Không nên sử dụng tài khoản Root cho các thao tác triển khai hằng ngày. Thay vào đó, nên tạo IAM User hoặc IAM Role riêng cho Workshop.

Các yêu cầu cần chuẩn bị:

- Tài khoản AWS đang hoạt động.
- Có quyền truy cập AWS Management Console.
- Đã bật xác thực đa yếu tố MFA.
- Có IAM User hoặc IAM Role dùng để triển khai.
- Đã thiết lập AWS Budget để theo dõi chi phí.

Workshop CyberNet được triển khai tại Region Singapore:

```text
ap-southeast-1
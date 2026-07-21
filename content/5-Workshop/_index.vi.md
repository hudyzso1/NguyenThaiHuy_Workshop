---
title: "Workshop"
date: 2026-07-14
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop CyberNet

## Tổng quan

Workshop này trình bày quá trình triển khai hệ thống **CyberNet quản lý phòng máy** trên AWS.

CyberNet được thiết kế nhằm hỗ trợ quản lý phòng máy thông qua kiến trúc Cloud. Hệ thống bao gồm triển khai Frontend, xử lý Backend API, tích hợp thanh toán, lưu trữ dữ liệu, giám sát, kiểm thử và dọn dẹp tài nguyên.

Workshop sử dụng các dịch vụ AWS sau:

- Amazon S3
- Amazon CloudFront
- Amazon VPC
- Amazon API Gateway
- AWS Lambda
- Amazon CloudWatch
- AWS IAM

Ngoài ra, hệ thống tích hợp với **PayOS** để hỗ trợ chức năng thanh toán.

---

## Nội dung Workshop

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Các bước chuẩn bị](5.2-Prerequisite/)
3. [Tạo VPC cho CyberNet](5.3-vpc/)
4. [Triển khai Frontend với Amazon S3 và CloudFront](5.4-Frontend-Amazon-S3-CloudFront/)
5. [Backend API và PayOS](5.5-backend-api-lambda-payos/)
6. [Quản lý dữ liệu trên S3](5.6-s3-data-management/)
7. [Kiểm thử và dọn dẹp](5.7-testing-cleanup/)

---

## Kết quả mong đợi

Sau khi hoàn thành Workshop, hệ thống CyberNet sẽ có:

- Hạ tầng mạng VPC cơ bản.
- Website Frontend được triển khai trên Amazon S3.
- Nội dung website được phân phối thông qua Amazon CloudFront.
- Backend API được xây dựng bằng API Gateway và Lambda.
- Tích hợp thanh toán với PayOS.
- Dữ liệu và tài nguyên được lưu trữ trên Amazon S3.
- Theo dõi hoạt động hệ thống bằng CloudWatch Logs.
- Quy trình dọn dẹp tài nguyên để kiểm soát chi phí AWS.
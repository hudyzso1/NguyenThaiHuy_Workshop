---
title: "Triển khai Frontend với Amazon S3 và CloudFront"
date: 2026-07-14
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---


Trong phần này, chúng ta sẽ triển khai giao diện Frontend của hệ thống **CyberNet** bằng **Amazon S3** và **Amazon CloudFront**.

Amazon S3 dùng để lưu trữ các file tĩnh của website. Amazon CloudFront dùng để phân phối nội dung website đến người dùng.

---

## 1. Tạo S3 Bucket

Mở **Amazon S3 Console** và chọn **Create bucket**.

Cấu hình bucket như sau:

| Thông số | Giá trị |
| :--- | :--- |
| **Bucket name** | `cybernet-frontend-bucket` |
| **Region** | `ap-southeast-1` |
| **Object Ownership** | ACLs disabled |
| **Block Public Access** | Enabled |

<img src="/NguyenThaiHuy_Workshop/5-Workshop/5.4-Frontend-Amazon-S3-CloudFront/1.png" alt="Tạo S3 Bucket" width="800">

---

## 2. Upload file Frontend lên S3

Mở bucket vừa tạo và chọn **Upload**.

<img src="/NguyenThaiHuy_Workshop/5-Workshop/5.4-Frontend-Amazon-S3-CloudFront/2.png" alt="Upload Frontend lên S3" width="800">

Upload các file Frontend của CyberNet:

```text
index.html
style.css
script.js
assets/
images/
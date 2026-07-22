---
title: "Tạo VPC cho CyberNet"
date: 2026-07-14
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---


Trong phần này, chúng ta sẽ tạo mạng Amazon VPC cơ bản cho hệ thống **CyberNet quản lý phòng máy**.

VPC được sử dụng làm nền tảng mạng cho CyberNet, giúp chuẩn bị cho việc mở rộng backend, cấu hình bảo mật và phân tách tài nguyên trên AWS.

---

## 1. Tạo VPC

Mở **Amazon VPC Console** và chọn **Create VPC**.

Cấu hình VPC như sau:

| Thông số | Giá trị |
| :--- | :--- |
| **Name tag** | `cybernet-vpc` |
| **IPv4 CIDR block** | `10.0.0.0/16` |
| **Region** | `ap-southeast-1` |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/vpc1.png" alt="Tạo CyberNet VPC" width="800">

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/vpc2.png" alt="Resource Map VPC" width="800">
---

## 2. Tạo Subnet

Tạo một public subnet và một private subnet cho CyberNet VPC.

| Tên Subnet | CIDR Block | Mục đích |
| :--- | :--- | :--- |
| `cybernet-public-subnet` | `10.0.1.0/24` | Tài nguyên public |
| `cybernet-private-subnet` | `10.0.2.0/24` | Tài nguyên backend riêng tư |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/subnet.png" alt="Resource Map VPC" width="800">

---

## 3. Tạo Internet Gateway

Tạo Internet Gateway và gắn vào CyberNet VPC.

| Tài nguyên | Tên |
| :--- | :--- |
| **Internet Gateway** | `cybernet-igw` |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/getway.png" alt="Resource Map VPC" width="800">

---

## 4. Cấu hình Route Table

Tạo public route table và thêm route ra Internet Gateway.

| Destination | Target |
| :--- | :--- |
| `10.0.0.0/16` | Local |
| `0.0.0.0/0` | `cybernet-igw` |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/route.png" alt="Resource Map VPC" width="800">

Liên kết route table này với:

```text
cybernet-public-subnet
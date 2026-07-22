---
title: "Create VPC for CyberNet"
date: 2026-07-14
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---



In this section, we will create the basic Amazon VPC network for the **CyberNet computer management system**.

The VPC is used as the network foundation for CyberNet, helping prepare for backend expansion, security configuration, and resource isolation on AWS.

---

## 1. Create VPC

Open the **Amazon VPC Console** and choose **Create VPC**.

Configure the VPC as follows:

| Parameter | Value |
| :--- | :--- |
| **Name tag** | `cybernet-vpc` |
| **IPv4 CIDR block** | `10.0.0.0/16` |
| **Region** | `ap-southeast-1` |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/vpc1.png" alt="Create CyberNet VPC" width="800">

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/vpc2.png" alt="VPC Resource Map" width="800">

---

## 2. Create Subnets

Create one public subnet and one private subnet for the CyberNet VPC.

| Subnet Name | CIDR Block | Purpose |
| :--- | :--- | :--- |
| `cybernet-public-subnet` | `10.0.1.0/24` | Public resources |
| `cybernet-private-subnet` | `10.0.2.0/24` | Private backend resources |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/subnet.png" alt="CyberNet Subnets" width="800">

---

## 3. Create Internet Gateway

Create an Internet Gateway and attach it to the CyberNet VPC.

| Resource | Name |
| :--- | :--- |
| **Internet Gateway** | `cybernet-igw` |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/getway.png" alt="CyberNet Internet Gateway" width="800">

---

## 4. Configure Route Table

Create a public route table and add a route to the Internet Gateway.

| Destination | Target |
| :--- | :--- |
| `10.0.0.0/16` | Local |
| `0.0.0.0/0` | `cybernet-igw` |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.3-S3-vpc/route.png" alt="CyberNet Route Table" width="800">

Associate this route table with:

```text
cybernet-public-subnet
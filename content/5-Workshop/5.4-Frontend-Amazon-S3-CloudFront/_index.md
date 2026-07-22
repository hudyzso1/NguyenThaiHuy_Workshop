---
title: "Deploy Frontend with Amazon S3 and CloudFront"
date: 2026-07-14
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

In this section, we will deploy the frontend interface of the **CyberNet** system using **Amazon S3** and **Amazon CloudFront**.

Amazon S3 is used to store static website files. Amazon CloudFront is used to distribute website content to users.

---

## 1. Create an S3 Bucket

Open the **Amazon S3 Console** and choose **Create bucket**.

Configure the bucket as follows:

| Parameter | Value |
| :--- | :--- |
| **Bucket name** | `cybernet-frontend-bucket` |
| **Region** | `ap-southeast-1` |
| **Object Ownership** | ACLs disabled |
| **Block Public Access** | Enabled |

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.4-Frontend-Amazon-S3-CloudFront/1.png" alt="Create S3 Bucket" width="800">

---

## 2. Upload Frontend Files to S3

Open the created bucket and choose **Upload**.

<img src="/NguyenThaiHuy_Workshop/images/5-Workshop/5.4-Frontend-Amazon-S3-CloudFront/2.png" alt="Upload Frontend Files to S3" width="800">

Upload the CyberNet frontend files:

```text
index.html
style.css
script.js
assets/
images/
```
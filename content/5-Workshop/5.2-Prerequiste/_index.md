---
title: "Prerequisites"
date: 2026-07-14
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---



Before deploying the **CyberNet computer management system** on AWS, several tools, permissions, and configurations need to be prepared.

CyberNet uses the following AWS services:

- Amazon S3
- Amazon CloudFront
- Amazon API Gateway
- AWS Lambda
- Amazon CloudWatch
- AWS IAM
- PayOS API

---

## 1. Prepare an AWS Account

First, prepare an AWS account that can be used to create and manage cloud resources.

For security reasons, the root account should not be used for daily deployment tasks. Instead, create an IAM user or IAM role for the workshop.

Required preparations:

- An active AWS account.
- AWS Management Console access.
- Multi-Factor Authentication enabled.
- An IAM user or role for deployment.
- AWS Budget configured to monitor costs.

The CyberNet workshop is deployed in the Singapore Region:

```text
ap-southeast-1
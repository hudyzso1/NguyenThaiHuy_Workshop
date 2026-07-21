---
title: "Workshop"
date: 2026-07-14
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# CyberNet Workshop

## Overview

This workshop presents the deployment process of the **CyberNet computer management system** on AWS.

CyberNet is designed to support computer room management through a cloud-based architecture. The system includes frontend deployment, backend API processing, payment integration, data storage, monitoring, testing, and resource cleanup.

The workshop uses the following AWS services:

- Amazon S3
- Amazon CloudFront
- Amazon VPC
- Amazon API Gateway
- AWS Lambda
- Amazon CloudWatch
- AWS IAM

In addition, the system integrates with **PayOS** to support the payment function.

---

## Workshop Content

1. [Introduction](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequisite/)
3. [Create VPC for CyberNet](5.3-vpc/)
4. [Deploy Frontend with Amazon S3 and CloudFront](5.4-Frontend-Amazon-S3-CloudFront/)
5. [Backend API and PayOS](5.5-backend-api-lambda-payos/)
6. [S3 Data Management](5.6-s3-data-management/)
7. [Testing and Cleanup](5.7-testing-cleanup/)

---

## Expected Outcome

After completing this workshop, the CyberNet system will have:

- A basic VPC network foundation.
- A frontend website deployed on Amazon S3.
- Content delivery through Amazon CloudFront.
- Backend APIs built with API Gateway and Lambda.
- Payment integration with PayOS.
- Data and resource storage on Amazon S3.
- Monitoring through CloudWatch Logs.
- A cleanup process to control AWS costs.
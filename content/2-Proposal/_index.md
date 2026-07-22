---
title: "Proposal"
date: 2024-07-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# CyberNet – Cloud  
## An AWS Serverless Solution for Modern Internet Café Management

### 1. Executive Summary

CyberNet – Cloud is an internet café management system built on AWS to modernize operations, including member management, computer usage time tracking, food ordering, online top-up/payment, and near real-time workstation screen monitoring.

The project aims to replace traditional local or manual management systems with a cloud-based architecture that is scalable, secure, and easier to operate. The system supports two main user groups: customers/members and administrators. Customers can log in, check their remaining play time, order food, make payments, and top up their account. Administrators can monitor machine status, manage members, process top-ups, track orders, and observe workstation activity.

The proposed architecture uses Amazon S3 and Amazon CloudFront for hosting and delivering the web interface, Amazon Cognito for authentication, Amazon API Gateway and AWS Lambda for business logic, Amazon DynamoDB for data storage, Amazon S3 for storing screen monitoring images, and Amazon VPC to isolate backend Lambda functions in private subnets.

---

### 2. Problem Statement

#### Current Problems

Many internet cafés still rely on local management software or manual operations. This creates several limitations:

- Difficult to centrally manage multiple workstations.
- Member data, balance, and play time may be lost if stored only locally.
- Administrators cannot easily monitor machine status in near real time.
- Top-up, payment, and order management are not fully automated.
- There is no built-in screen monitoring mechanism for management and auditing.
- The system is difficult to scale when the number of workstations or users increases.

#### Proposed Solution

CyberNet – Cloud proposes an AWS Serverless architecture combined with Amazon VPC. Users access the web application through CloudFront and S3. Registration, login, and role-based access control are handled by Amazon Cognito. Business requests such as member management, machine management, play time, food ordering, and payment are sent through API Gateway to AWS Lambda.

Core system data is stored in Amazon DynamoDB, including members, machine status, play time, orders, and payment transactions. Screen monitoring images are stored in Amazon S3. Lambda functions run inside private subnets within an Amazon VPC to improve network isolation. The Payment Lambda uses a NAT Gateway to communicate with PayOS when creating payment links or verifying transactions.

#### Benefits and Business Value

- Automates member and workstation management.
- Reduces manual work for administrators.
- Allows customers to top up and order food by themselves.
- Makes play time management more transparent.
- Stores operational data centrally on AWS.
- Supports future scaling when the number of machines increases.
- Improves monitoring and control of internet café operations.
- Provides a foundation for future features such as revenue analytics, anomaly alerts, food inventory management, and real-time customer support chat.

---

### 3. Solution Architecture

CyberNet – Cloud applies a Serverless architecture on AWS combined with Amazon VPC to ensure scalability, security, and operational efficiency.

The high-level system flow is as follows:

1. Customers or administrators access the website through Amazon CloudFront.
2. CloudFront delivers the static web interface stored in Amazon S3.
3. Users register, log in, and receive authentication tokens through Amazon Cognito.
4. The frontend sends requests to Amazon API Gateway.
5. API Gateway validates access permissions and forwards requests to AWS Lambda.
6. Lambda processes business logic such as members, machines, play time, menu, orders, and payments.
7. System data is stored in Amazon DynamoDB.
8. Workstations send screen monitoring images or metadata to the system.
9. Screen Lambda generates presigned URLs so workstations can upload images directly to Amazon S3.
10. The Admin Dashboard retrieves machine status and the latest screenshots for monitoring.
11. Payment Lambda integrates with PayOS to create payment links and handle webhooks.
12. Amazon CloudWatch collects logs, metrics, and system alerts.

![CyberNet Cloud Architecture](/NguyenThaiHuy_Workshop/images/2-Proposal/cybernet_architecture.png)

#### AWS Services Used

| AWS Service | Role in the System |
|---|---|
| Amazon S3 | Stores the frontend website and screen monitoring images |
| Amazon CloudFront | Delivers the website securely and quickly via HTTPS |
| Amazon Cognito | Manages user registration, login, and role-based access |
| Amazon API Gateway | Receives REST API requests from the frontend, workstations, and PayOS webhook |
| AWS Lambda | Handles backend business logic |
| Amazon DynamoDB | Stores members, machines, orders, payments, and system status |
| Amazon VPC | Isolates Lambda functions inside private subnets |
| NAT Gateway | Allows Lambda functions in private subnets to call PayOS |
| VPC Endpoint | Allows private access from Lambda to S3 and DynamoDB |
| AWS Secrets Manager | Stores PayOS API keys and sensitive configuration |
| AWS IAM | Manages access permissions using the least privilege principle |
| Amazon CloudWatch | Monitors logs, metrics, and system errors |

#### Component Design

- **Frontend Layer**: The web interface includes the login page, client dashboard, and admin dashboard. It is stored in Amazon S3 and delivered through CloudFront.
- **Authentication Layer**: Amazon Cognito handles user registration, login, and role-based access control between admin and client users.
- **API Layer**: API Gateway acts as the entry point for requests from the frontend, workstations, and PayOS.
- **Compute Layer**: AWS Lambda processes the main business logic, payment flow, and screen monitoring operations.
- **Network Layer**: Lambda functions are placed inside private subnets. NAT Gateway is used for outbound traffic to PayOS.
- **Data Layer**: DynamoDB stores system data, while S3 stores screen monitoring images.
- **Security Layer**: IAM, Secrets Manager, and VPC Endpoints help control access and protect sensitive data.
- **Monitoring Layer**: CloudWatch monitors logs, errors, Lambda performance, and API Gateway activity.

---

### 4. Technical Implementation

#### Implementation Phases

The project is divided into four main phases:

1. **Requirement Analysis and Architecture Design**
   - Identify the main system features.
   - Design the AWS architecture diagram.
   - Determine the required AWS services.
   - Design the data flow between frontend, API, Lambda, DynamoDB, and S3.

2. **Initial Frontend and Backend Development**
   - Build the login, registration, client dashboard, and admin dashboard interfaces.
   - Build APIs for members, machines, play time, orders, and payments.
   - Test the core functions in the local environment.

3. **AWS Deployment**
   - Upload the frontend to Amazon S3.
   - Configure CloudFront.
   - Create a Cognito User Pool.
   - Build API Gateway routes.
   - Convert backend logic to AWS Lambda.
   - Design DynamoDB tables.
   - Configure an S3 bucket for screen image storage.
   - Configure VPC, private subnets, NAT Gateway, and VPC Endpoints.

4. **Testing, Optimization, and Finalization**
   - Test login, top-up, food ordering, and screen monitoring flows.
   - Test the PayOS webhook.
   - Configure CloudWatch logs and alarms.
   - Review IAM permissions.
   - Optimize cost and performance.
   - Prepare the final report and demo.

#### Technical Requirements

- **Frontend**
  - HTML, CSS, and JavaScript.
  - Interfaces for customers and administrators.
  - HTTPS connection to API Gateway.

- **Backend**
  - AWS Lambda for business logic.
  - API Gateway for request routing.
  - Cognito Authorizer for token validation.

- **Database**
  - DynamoDB stores member information, machine status, orders, transactions, and play time.

- **Payment**
  - PayOS integration.
  - Payment Lambda creates payment links.
  - Webhook verifies transactions and updates account balance.

- **Screen Monitoring**
  - Workstations request a presigned URL.
  - The latest screen image is uploaded to S3.
  - The Admin Dashboard displays the latest image of each workstation.

- **Networking and Security**
  - Lambda functions are placed in private subnets.
  - NAT Gateway is used for outbound traffic to PayOS.
  - S3 and DynamoDB are accessed through VPC Endpoints.
  - Secrets Manager stores sensitive information.
  - IAM applies the least privilege principle.

---

### 5. Roadmap and Milestones

| Phase | Description | Estimated Time |
|---|---|---|
| Phase 1 | Analyze requirements, define features, and design architecture | Week 1 |
| Phase 2 | Build client, admin, and lock screen interfaces | Week 2 |
| Phase 3 | Build business APIs and database structure | Week 3 |
| Phase 4 | Integrate PayOS and process transactions | Week 4 |
| Phase 5 | Build screen monitoring functionality | Week 5 |
| Phase 6 | Deploy frontend to S3 and CloudFront | Week 6 |
| Phase 7 | Deploy Lambda, API Gateway, DynamoDB, and S3 storage | Week 7 |
| Phase 8 | Configure VPC, NAT Gateway, VPC Endpoints, IAM, and Secrets Manager | Week 8 |
| Phase 9 | Test, optimize, complete report, and prepare demo | Week 9 |

---

### 6. Budget Estimation

The following cost estimate is for a small demo/MVP environment with approximately 5–15 workstations and low traffic.

| Service | Purpose | Estimated Monthly Cost |
|---|---|---:|
| Amazon S3 | Stores frontend and screen images | 1–3 USD |
| Amazon CloudFront | Delivers the website | 0–2 USD |
| Amazon Cognito | Manages users | 0 USD at small scale |
| Amazon API Gateway | Handles REST API requests | 1–3 USD |
| AWS Lambda | Processes business logic | 0–2 USD |
| Amazon DynamoDB | Stores system data | 1–5 USD |
| NAT Gateway | Allows Lambda to call PayOS from private subnets | 30–35 USD |
| VPC Endpoint | Provides private access to S3 and DynamoDB | 0–5 USD |
| AWS Secrets Manager | Stores PayOS API keys | Around 0.40 USD per secret |
| Amazon CloudWatch | Logs and metrics | 1–5 USD |

**Estimated total:** approximately **35–60 USD/month** if NAT Gateway is used.

For a cost-optimized demo environment, Lambda can temporarily run outside the VPC when calling PayOS, or the NAT Gateway can be optimized. However, for an enterprise-oriented VPC architecture, using NAT Gateway is the more standard approach.

---

### 7. Risk Assessment

#### Risk Matrix

| Risk | Impact | Probability | Severity |
|---|---|---|---|
| PayOS API keys are exposed | High | Medium | High |
| Payment webhook is forged | High | Medium | High |
| Workstations upload oversized images | Medium | High | High |
| Poor DynamoDB key design | Medium | Medium | Medium |
| NAT Gateway increases monthly cost | Medium | High | High |
| Users modify frontend-side data | High | Medium | High |
| Workstation Internet connection is lost | Medium | Medium | Medium |
| Admin screen monitoring is slow due to frequent polling | Medium | Medium | Medium |

#### Mitigation Strategies

- Store PayOS keys in AWS Secrets Manager.
- Verify PayOS webhook signatures before updating transactions.
- Use DynamoDB conditional writes to prevent duplicate transaction processing.
- Do not trust data stored in localStorage; all balances, orders, and payments must be verified by the backend.
- Limit uploaded image size.
- Use WebP or JPEG instead of Base64 PNG.
- Use presigned URLs to upload images directly to S3.
- Configure IAM using the least privilege principle.
- Monitor CloudWatch Logs and create alarms for Lambda and API Gateway errors.
- Optimize NAT Gateway cost or separate demo and production environments.

#### Contingency Plan

- If PayOS is unavailable, administrators can manually top up accounts at the counter.
- If S3 image upload fails, the workstation can continue operating and retry the upload later.
- If DynamoDB has a temporary issue, the system displays a maintenance message and disables new transactions.
- If CloudFront cache causes issues, the cache can be invalidated or rolled back to a previous frontend version.
- If Lambda fails, CloudWatch Logs can be used for troubleshooting and the Lambda version can be rolled back.

---

### 8. Expected Outcomes

#### Technical Improvements

CyberNet – Cloud helps migrate internet café management from a local system to a centralized cloud platform. The system can handle login, member management, play time tracking, food ordering, payment, and screen monitoring in one platform.

#### Operational Value

Administrators can monitor machine status more easily and reduce manual effort when processing top-ups or checking workstations. Customers can log in, check their remaining time, order food, and top up quickly.

#### Long-Term Value

The project provides a foundation for future enhancements such as:

- Daily and monthly revenue analytics.
- Food inventory management.
- Alerts when a machine is about to run out of time.
- Real-time support chat between customers and administrators.
- User behavior analytics.
- Multi-branch internet café management.
- Advanced administration dashboard.

CyberNet – Cloud is the first step toward building a modern, scalable, and cloud-ready internet café management system.
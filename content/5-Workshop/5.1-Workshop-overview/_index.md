---
title : "Introduction"
date : 2026-07-21
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction to CyberNet Cloud

**CyberNet Cloud** is a cloud-based internet café management system deployed on Amazon Web Services (AWS). The system is designed to support both administrators and client machines in a cyber game room environment. It allows customers to log in to a rented machine, view their remaining balance, order food and drinks, chat with the administrator, and make payments through PayOS. On the administrator side, the system supports machine management, manual balance top-up, product management, food order processing, customer chat, real-time updates, and remote screen monitoring through an edge screen agent.

The project follows a serverless and cloud-native approach to reduce infrastructure management effort, improve scalability, and separate frontend, backend, database, payment, and monitoring components. By using AWS services such as Amazon S3, Amazon CloudFront, AWS Lambda, Amazon API Gateway, Amazon DynamoDB, AWS WAF, and API Gateway WebSocket, CyberNet Cloud can operate as a flexible management platform for internet cafés without requiring a traditional physical server.

#### Architecture overview

The CyberNet Cloud frontend is built with HTML, CSS, and JavaScript, then hosted on Amazon S3 and delivered to users through Amazon CloudFront. CloudFront improves access speed and provides a secure public entry point for both the admin dashboard and client interface. AWS WAF is associated with CloudFront to help protect the application from common web attacks and suspicious requests.

All frontend requests are sent to Amazon API Gateway over HTTPS. API Gateway forwards requests to AWS Lambda, which acts as the main backend service of the system. The backend handles business logic such as machine authentication, balance management, product management, order processing, PayOS payment synchronization, chat messages, and screen monitoring APIs. Data is stored in Amazon DynamoDB tables, including machines, products, orders, chat messages, and WebSocket connections. Product images and screen capture images are stored securely in Amazon S3.

#### Authentication and Machine Management

CyberNet Cloud provides separate access flows for administrators and client machines. Client machines such as **MAY01**, **MAY02**, and **MAY03** can log in using machine accounts. The administrator account is used to access the admin dashboard and manage the entire system. After login, the client interface displays the current machine name, remaining balance, remaining usage time, food menu, shopping cart, chat box, and PayOS payment functions.

The rental timer is calculated based on the customer’s remaining balance. In this system, **15,000 VND equals 1 hour of usage time**. For example, if a customer has 40,000 VND in the account balance, the system calculates the remaining usage time and displays it on the client interface. The backend periodically synchronizes the timer with DynamoDB to ensure that the customer balance and remaining time stay consistent across the cloud system. When the balance reaches zero, the client machine is automatically logged out.

#### Payment and Food Ordering

CyberNet Cloud integrates **PayOS** to support online payment. Customers can top up their machine balance through PayOS, and the backend uses PayOS payment synchronization to verify successful transactions before updating the customer’s balance in DynamoDB. This prevents the system from adding money before a payment is confirmed.

The food ordering flow is also integrated with PayOS. The correct workflow is: the customer selects food items, adds them to the cart, clicks the order button, and the system immediately creates a PayOS payment link or QR code. After the customer completes the payment, the order is synchronized and shown in the admin dashboard with product names, quantities, total price, and payment status. Food orders do not increase the machine balance; they are stored as paid orders for the administrator to process.

#### Realtime Communication and Monitoring

CyberNet Cloud uses API Gateway WebSocket to support realtime updates between the client interface and the admin dashboard. When customers place orders, send chat messages, or when administrators update machine information, realtime signals are sent through WebSocket connections so that the interface can refresh automatically without requiring manual page reloads.

The system also includes a screen monitoring feature. A local edge screen agent runs on client machines and captures screen images periodically. These screen captures are uploaded to Amazon S3 and referenced through backend APIs. The admin dashboard can view the latest screen image of each client machine, update the screen manually, and enable or disable screen monitoring for specific machines. This design avoids exposing the S3 bucket publicly while still allowing administrators to monitor client machines through the cloud dashboard.

#### Main workshop contents

In this workshop, you will deploy and configure the main AWS components required by CyberNet Cloud. First, the frontend will be uploaded to Amazon S3 and distributed through Amazon CloudFront. AWS WAF will be configured to protect the public website. Next, Amazon API Gateway and AWS Lambda will be used to build the backend API for machine login, product management, order management, chat, PayOS payment, and screen monitoring.

Amazon DynamoDB will be used to store machines, products, orders, chat messages, and WebSocket connection data. Amazon S3 will store product images and screen capture files. The workshop also covers the configuration of PayOS payment flow, realtime WebSocket communication, rental timer synchronization, and admin dashboard functions. Finally, the system will be tested through complete user flows, including client login, balance top-up, food ordering, PayOS payment, chat communication, screen monitoring, and automatic logout when the customer balance runs out.

#### CyberNet Cloud overall architecture

![CyberNet Cloud Overall Architecture](/images/2-Proposal/cybernet-architecture.png)
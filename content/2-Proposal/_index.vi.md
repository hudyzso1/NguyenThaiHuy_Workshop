---
title: "Bản đề xuất"
date: 2026-07-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# CyberNet – Cloud  
## Giải pháp AWS Serverless cho quản lý phòng máy tiệm net hiện đại

### 1. Tóm tắt điều hành

CyberNet – Cloud là hệ thống quản lý phòng máy tiệm net được xây dựng trên nền tảng AWS nhằm hiện đại hóa quy trình vận hành, quản lý hội viên, quản lý thời gian sử dụng máy, đặt món ăn, nạp tiền trực tuyến và giám sát màn hình máy trạm theo thời gian gần thực.

Dự án hướng đến việc thay thế mô hình quản lý thủ công hoặc phần mềm cục bộ truyền thống bằng một kiến trúc Cloud có khả năng mở rộng, bảo mật và dễ quản trị. Hệ thống hỗ trợ hai nhóm người dùng chính gồm khách hàng/hội viên và quản lý/admin. Khách hàng có thể đăng nhập, theo dõi thời gian còn lại, đặt món, thanh toán và nạp tiền. Admin có thể theo dõi trạng thái máy, quản lý hội viên, xử lý nạp tiền, kiểm tra đơn hàng và giám sát hoạt động máy trạm.

Kiến trúc đề xuất sử dụng Amazon S3 và Amazon CloudFront cho giao diện web, Amazon Cognito cho xác thực, Amazon API Gateway và AWS Lambda cho xử lý nghiệp vụ, Amazon DynamoDB cho lưu trữ dữ liệu, Amazon S3 cho lưu ảnh giám sát màn hình, cùng Amazon VPC để cô lập các Lambda xử lý nghiệp vụ trong private subnet.

---

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Nhiều phòng máy tiệm net vẫn phụ thuộc vào hệ thống quản lý cục bộ hoặc thao tác thủ công. Điều này dẫn đến một số hạn chế:

- Khó quản lý tập trung nhiều máy trạm.
- Dữ liệu hội viên, số dư và thời gian chơi dễ bị thất lạc nếu chỉ lưu cục bộ.
- Admin khó theo dõi trạng thái máy theo thời gian thực.
- Việc nạp tiền, thanh toán và quản lý đơn hàng chưa được tự động hóa hoàn toàn.
- Thiếu cơ chế giám sát màn hình máy trạm phục vụ quản lý và kiểm tra.
- Khả năng mở rộng kém khi số lượng máy hoặc người dùng tăng lên.

#### Giải pháp

CyberNet – Cloud đề xuất xây dựng hệ thống quản lý phòng máy dựa trên kiến trúc AWS Serverless kết hợp Amazon VPC. Người dùng truy cập giao diện thông qua CloudFront và S3. Quá trình đăng ký, đăng nhập và phân quyền được xử lý bằng Amazon Cognito. Các request nghiệp vụ như quản lý hội viên, máy trạm, đơn hàng, thời gian chơi và thanh toán được gửi qua API Gateway đến AWS Lambda.

Dữ liệu chính của hệ thống được lưu trong DynamoDB, bao gồm thông tin hội viên, trạng thái máy, thời gian chơi, đơn hàng và giao dịch thanh toán. Hình ảnh giám sát màn hình được lưu trên Amazon S3. Các Lambda chạy trong private subnet của VPC để tăng tính cô lập mạng. Payment Lambda sử dụng NAT Gateway để gọi ra ngoài đến PayOS khi cần tạo link thanh toán hoặc xác minh giao dịch.

#### Lợi ích và giá trị mang lại

- Tự động hóa quy trình quản lý hội viên và máy trạm.
- Giảm thao tác thủ công cho admin.
- Hỗ trợ khách hàng tự nạp tiền và đặt món.
- Quản lý thời gian chơi minh bạch hơn.
- Dữ liệu được lưu trữ tập trung trên AWS.
- Dễ mở rộng khi tăng số lượng máy trạm.
- Tăng khả năng giám sát và kiểm soát hoạt động phòng máy.
- Tạo nền tảng để phát triển thêm các chức năng như thống kê doanh thu, cảnh báo bất thường, quản lý kho đồ ăn và chat hỗ trợ realtime.

---

### 3. Kiến trúc giải pháp

CyberNet – Cloud áp dụng kiến trúc Serverless trên AWS kết hợp VPC nhằm đảm bảo khả năng mở rộng, bảo mật và dễ vận hành.

Luồng tổng quan của hệ thống:

1. Khách hàng hoặc admin truy cập website thông qua Amazon CloudFront.
2. CloudFront phân phối giao diện tĩnh được lưu trên Amazon S3.
3. Người dùng đăng ký, đăng nhập và nhận token xác thực thông qua Amazon Cognito.
4. Frontend gửi request đến Amazon API Gateway.
5. API Gateway kiểm tra quyền truy cập và chuyển request đến AWS Lambda.
6. Lambda xử lý nghiệp vụ như hội viên, máy trạm, thời gian chơi, menu, đơn hàng và thanh toán.
7. Dữ liệu được lưu vào Amazon DynamoDB.
8. Máy trạm gửi ảnh màn hình hoặc metadata giám sát lên hệ thống.
9. Screen Lambda cấp presigned URL để máy trạm upload ảnh trực tiếp vào Amazon S3.
10. Admin Dashboard lấy trạng thái máy và ảnh mới nhất để phục vụ giám sát.
11. Payment Lambda tích hợp PayOS để tạo link thanh toán và xử lý webhook.
12. CloudWatch ghi log, metrics và cảnh báo lỗi hệ thống.
![CyberNet Cloud Architecture](/NguyenThaiHuy_Workshop/images/2-Proposal/cybernet-architecture.png)

#### Dịch vụ AWS sử dụng

| Dịch vụ AWS | Vai trò trong hệ thống |
|---|---|
| Amazon S3 | Lưu giao diện frontend và ảnh giám sát màn hình |
| Amazon CloudFront | Phân phối website nhanh và an toàn qua HTTPS |
| Amazon Cognito | Quản lý đăng ký, đăng nhập và phân quyền người dùng |
| Amazon API Gateway | Tiếp nhận REST API request từ frontend, máy trạm và PayOS webhook |
| AWS Lambda | Xử lý logic nghiệp vụ của hệ thống |
| Amazon DynamoDB | Lưu dữ liệu hội viên, máy trạm, đơn hàng, giao dịch và trạng thái |
| Amazon VPC | Cô lập các Lambda trong private subnet |
| NAT Gateway | Cho Lambda trong private subnet gọi ra ngoài đến PayOS |
| VPC Endpoint | Cho Lambda truy cập S3 và DynamoDB qua mạng riêng AWS |
| AWS Secrets Manager | Lưu trữ PayOS API key và thông tin nhạy cảm |
| AWS IAM | Phân quyền truy cập theo nguyên tắc least privilege |
| Amazon CloudWatch | Theo dõi logs, metrics và cảnh báo lỗi |

#### Thiết kế thành phần

- **Frontend Layer**: Giao diện web gồm trang đăng nhập, trang hội viên và trang admin được lưu trên Amazon S3 và phân phối qua CloudFront.
- **Authentication Layer**: Amazon Cognito xử lý đăng ký, đăng nhập và phân quyền giữa admin và client.
- **API Layer**: API Gateway là cổng tiếp nhận request từ frontend, máy trạm và PayOS.
- **Compute Layer**: AWS Lambda xử lý nghiệp vụ chính, thanh toán và giám sát màn hình.
- **Network Layer**: Lambda được đặt trong private subnet của Amazon VPC. NAT Gateway hỗ trợ kết nối outbound đến PayOS.
- **Data Layer**: DynamoDB lưu dữ liệu hệ thống, S3 lưu ảnh giám sát màn hình.
- **Security Layer**: IAM, Secrets Manager và VPC Endpoint giúp kiểm soát quyền truy cập và bảo vệ dữ liệu.
- **Monitoring Layer**: CloudWatch theo dõi log, lỗi, hiệu năng Lambda và API Gateway.

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án được chia thành 4 giai đoạn chính:

1. **Phân tích yêu cầu và thiết kế kiến trúc**
   - Xác định chức năng chính của hệ thống.
   - Thiết kế sơ đồ kiến trúc AWS.
   - Xác định các dịch vụ AWS cần sử dụng.
   - Thiết kế luồng dữ liệu giữa frontend, API, Lambda, DynamoDB và S3.

2. **Xây dựng giao diện và backend ban đầu**
   - Xây dựng giao diện đăng nhập, đăng ký, client dashboard và admin dashboard.
   - Xây dựng API xử lý hội viên, máy trạm, thời gian chơi, đơn hàng và thanh toán.
   - Kiểm thử các chức năng cơ bản ở môi trường local.

3. **Triển khai lên AWS**
   - Upload frontend lên Amazon S3.
   - Cấu hình CloudFront.
   - Tạo Cognito User Pool.
   - Xây dựng API Gateway.
   - Chuyển logic backend sang AWS Lambda.
   - Thiết kế bảng DynamoDB.
   - Cấu hình S3 bucket lưu ảnh màn hình.
   - Cấu hình VPC, private subnet, NAT Gateway và VPC Endpoint.

4. **Kiểm thử, tối ưu và hoàn thiện**
   - Kiểm thử luồng đăng nhập, nạp tiền, đặt món và giám sát màn hình.
   - Kiểm thử PayOS webhook.
   - Cấu hình CloudWatch log và alarm.
   - Kiểm tra quyền IAM.
   - Tối ưu chi phí và hiệu năng.
   - Chuẩn bị báo cáo và demo.

#### Yêu cầu kỹ thuật

- **Frontend**:
  - HTML, CSS, JavaScript.
  - Giao diện cho khách hàng và admin.
  - Kết nối API Gateway thông qua HTTPS.

- **Backend**:
  - AWS Lambda xử lý nghiệp vụ.
  - API Gateway định tuyến request.
  - Cognito Authorizer kiểm tra token người dùng.

- **Database**:
  - DynamoDB lưu thông tin hội viên, máy trạm, đơn hàng, giao dịch và thời gian chơi.

- **Thanh toán**:
  - Tích hợp PayOS.
  - Payment Lambda tạo link thanh toán.
  - Webhook xác minh giao dịch và cập nhật số dư.

- **Giám sát màn hình**:
  - Máy trạm gửi yêu cầu tạo presigned URL.
  - Upload ảnh mới nhất lên S3.
  - Admin Dashboard xem ảnh mới nhất của từng máy.

- **Mạng và bảo mật**:
  - Lambda đặt trong private subnet.
  - NAT Gateway dùng cho outbound traffic đến PayOS.
  - S3 và DynamoDB truy cập qua VPC Endpoint.
  - Secrets Manager lưu thông tin nhạy cảm.
  - IAM phân quyền theo least privilege.

---

### 5. Lộ trình và mốc triển khai

| Giai đoạn | Nội dung | Thời gian dự kiến |
|---|---|---|
| Giai đoạn 1 | Phân tích yêu cầu, xác định chức năng, thiết kế kiến trúc | Tuần 1 |
| Giai đoạn 2 | Xây dựng giao diện client, admin và lock screen | Tuần 2 |
| Giai đoạn 3 | Xây dựng API nghiệp vụ và cơ sở dữ liệu | Tuần 3 |
| Giai đoạn 4 | Tích hợp PayOS và xử lý giao dịch | Tuần 4 |
| Giai đoạn 5 | Xây dựng chức năng giám sát màn hình | Tuần 5 |
| Giai đoạn 6 | Triển khai frontend lên S3 và CloudFront | Tuần 6 |
| Giai đoạn 7 | Triển khai Lambda, API Gateway, DynamoDB và S3 storage | Tuần 7 |
| Giai đoạn 8 | Cấu hình VPC, NAT Gateway, VPC Endpoint, IAM và Secrets Manager | Tuần 8 |
| Giai đoạn 9 | Kiểm thử, tối ưu, hoàn thiện báo cáo và demo | Tuần 9 |

---

### 6. Ước tính ngân sách

Chi phí dưới đây là ước tính cho môi trường demo/MVP với quy mô nhỏ, khoảng 5–15 máy trạm và lưu lượng thấp.

| Dịch vụ | Mục đích | Ước tính chi phí/tháng |
|---|---|---:|
| Amazon S3 | Lưu frontend và ảnh màn hình | 1–3 USD |
| Amazon CloudFront | Phân phối website | 0–2 USD |
| Amazon Cognito | Quản lý người dùng | 0 USD ở quy mô nhỏ |
| Amazon API Gateway | REST API request | 1–3 USD |
| AWS Lambda | Xử lý nghiệp vụ | 0–2 USD |
| Amazon DynamoDB | Lưu dữ liệu hệ thống | 1–5 USD |
| NAT Gateway | Lambda gọi PayOS từ private subnet | 30–35 USD |
| VPC Endpoint | Truy cập riêng S3/DynamoDB | 0–5 USD |
| AWS Secrets Manager | Lưu PayOS API key | Khoảng 0,40 USD/secret |
| Amazon CloudWatch | Logs và metrics | 1–5 USD |

**Tổng ước tính:** khoảng **35–60 USD/tháng** nếu sử dụng NAT Gateway.

Đối với môi trường demo tiết kiệm chi phí, có thể tạm thời để Lambda không nằm trong VPC khi gọi PayOS hoặc dùng phương án giảm NAT Gateway. Tuy nhiên, với kiến trúc doanh nghiệp có VPC private subnet, NAT Gateway vẫn là lựa chọn chuẩn hơn.

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro | Ảnh hưởng | Xác suất | Mức độ |
|---|---|---|---|
| Lộ API key PayOS | Cao | Trung bình | Cao |
| Webhook thanh toán bị gọi giả | Cao | Trung bình | Cao |
| Máy trạm gửi ảnh quá lớn | Trung bình | Cao | Cao |
| DynamoDB thiết kế khóa chưa tốt | Trung bình | Trung bình | Trung bình |
| NAT Gateway làm tăng chi phí | Trung bình | Cao | Cao |
| Người dùng sửa dữ liệu phía frontend | Cao | Trung bình | Cao |
| Mất kết nối Internet ở máy trạm | Trung bình | Trung bình | Trung bình |
| Admin xem ảnh chậm do polling liên tục | Trung bình | Trung bình | Trung bình |

#### Chiến lược giảm thiểu

- Lưu PayOS key trong AWS Secrets Manager.
- Xác minh chữ ký webhook PayOS trước khi cập nhật giao dịch.
- Dùng DynamoDB conditional write để tránh xử lý trùng giao dịch.
- Không tin dữ liệu từ localStorage; mọi số dư, đơn hàng và thanh toán phải xác minh ở backend.
- Giới hạn kích thước ảnh upload.
- Dùng định dạng WebP/JPEG thay vì Base64 PNG.
- Dùng presigned URL để upload ảnh trực tiếp lên S3.
- Cấu hình IAM theo nguyên tắc least privilege.
- Theo dõi CloudWatch Logs và thiết lập alarm cho lỗi Lambda/API Gateway.
- Tối ưu chi phí NAT Gateway hoặc tách môi trường demo và production.

#### Kế hoạch dự phòng

- Nếu PayOS gặp sự cố, admin có thể nạp tiền thủ công tại quầy.
- Nếu S3 upload ảnh lỗi, máy trạm vẫn có thể tiếp tục hoạt động và gửi lại ảnh sau.
- Nếu DynamoDB lỗi tạm thời, hệ thống hiển thị thông báo bảo trì và không cho tạo giao dịch mới.
- Nếu CloudFront lỗi cache, có thể invalidate cache hoặc rollback phiên bản frontend trước đó.
- Nếu Lambda lỗi, kiểm tra CloudWatch Logs và rollback phiên bản Lambda ổn định.

---

### 8. Kết quả kỳ vọng

#### Cải tiến kỹ thuật

CyberNet – Cloud giúp chuyển hệ thống quản lý phòng máy từ mô hình local sang mô hình Cloud tập trung. Hệ thống có khả năng xử lý đăng nhập, quản lý hội viên, thời gian chơi, đặt món, thanh toán và giám sát màn hình trên cùng một nền tảng.

#### Giá trị vận hành

Admin có thể theo dõi trạng thái phòng máy dễ dàng hơn, giảm thao tác thủ công khi nạp tiền hoặc kiểm tra máy trạm. Khách hàng có thể tự đăng nhập, theo dõi thời gian, đặt món và nạp tiền nhanh chóng.

#### Giá trị dài hạn

Dự án tạo nền tảng để phát triển thêm các chức năng mở rộng như:

- Thống kê doanh thu theo ngày/tháng.
- Quản lý tồn kho đồ ăn.
- Cảnh báo máy sắp hết giờ.
- Chat hỗ trợ realtime giữa khách hàng và admin.
- Phân tích hành vi sử dụng máy.
- Quản lý nhiều chi nhánh phòng máy.
- Dashboard quản trị nâng cao.

CyberNet – Cloud là bước khởi đầu để xây dựng một hệ thống quản lý phòng máy hiện đại, có khả năng mở rộng và phù hợp với xu hướng triển khai ứng dụng trên nền tảng Cloud.
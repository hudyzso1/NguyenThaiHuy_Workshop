---
title: "Quản lý dữ liệu trên S3"
date: 2026-07-14
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---


Trong phần này, chúng ta sẽ sử dụng **Amazon S3** để quản lý dữ liệu và tài nguyên của hệ thống **CyberNet**.

Ngoài việc lưu trữ Frontend, Amazon S3 còn có thể được sử dụng để lưu hình ảnh sản phẩm, file tĩnh và các tài nguyên được hệ thống upload trong quá trình hoạt động.

---

## 1. Tạo thư mục lưu trữ tài nguyên

Mở S3 Bucket của CyberNet và tạo các thư mục dùng để quản lý dữ liệu.

Ví dụ:

```text
products/
images/
uploads/
```

Các thư mục này giúp hệ thống dễ dàng phân loại tài nguyên trong quá trình sử dụng.

---

## 2. Upload hình ảnh sản phẩm

Chọn thư mục `products/` và upload hình ảnh sản phẩm lên S3.

Các file hình ảnh có thể bao gồm:

```text
product-1.png
product-2.png
product-3.png
```

Sau khi upload, kiểm tra lại danh sách object trong bucket để đảm bảo file đã được lưu thành công.

---

## 3. Kiểm tra đường dẫn object

Sau khi upload file, mở chi tiết object để kiểm tra thông tin file.

Các thông tin cần kiểm tra gồm:

| Thông tin | Mục đích |
| :--- | :--- |
| **Object name** | Kiểm tra tên file |
| **Object URL** | Dùng để tham chiếu tài nguyên |
| **Size** | Kiểm tra dung lượng file |
| **Last modified** | Kiểm tra thời gian upload |

---

## 4. Cấu hình quyền truy cập

Trong hệ thống CyberNet, các file lưu trên S3 cần được kiểm soát quyền truy cập phù hợp.

Một số lưu ý:

- Không để lộ file nhạy cảm công khai.
- Chỉ cho phép truy cập các file cần hiển thị cho người dùng.
- Không upload Access Key, Secret Key hoặc file `.env` lên S3.
- Có thể sử dụng CloudFront để phân phối nội dung từ S3.

---

## 5. Kết nối S3 với Backend

Backend có thể sử dụng đường dẫn file trên S3 để lưu thông tin hình ảnh sản phẩm hoặc tài nguyên liên quan.

Ví dụ dữ liệu sản phẩm có thể lưu kèm đường dẫn ảnh:

```json
{
  "name": "Mì trộn",
  "price": 15000,
  "stock": 10,
  "imageKey": "products/product-1.png",
  "imageUrl": "https://example-cloudfront-domain/products/product-1.png"
}
```

Khi Frontend hiển thị sản phẩm, hệ thống sẽ sử dụng `imageUrl` để tải hình ảnh từ S3 hoặc CloudFront.

---

## Kết quả

Sau khi hoàn thành phần này, hệ thống CyberNet có thể:

- Lưu trữ hình ảnh sản phẩm trên Amazon S3.
- Quản lý file tĩnh và tài nguyên upload.
- Phân loại object theo thư mục.
- Sử dụng đường dẫn S3 hoặc CloudFront trong dữ liệu sản phẩm.
- Chuẩn bị dữ liệu để kết nối với Backend và Frontend.
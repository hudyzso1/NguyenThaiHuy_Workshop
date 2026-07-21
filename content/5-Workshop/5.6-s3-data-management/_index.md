---
title: "S3 Data Management"
date: 2026-07-14
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---


In this section, we will use **Amazon S3** to manage data and resources for the **CyberNet** system.

Besides storing the Frontend, Amazon S3 can also be used to store product images, static files, and uploaded resources during system operation.

---

## 1. Create Folders for Resources

Open the CyberNet S3 Bucket and create folders to manage data.

Example:

```text
products/
images/
uploads/
```

These folders help the system organize resources more clearly during usage.

---

## 2. Upload Product Images

Choose the `products/` folder and upload product images to S3.

Example image files:

```text
product-1.png
product-2.png
product-3.png
```

After uploading, check the object list in the bucket to make sure the files are stored successfully.

---

## 3. Check Object Information

After uploading a file, open the object details page to check its information.

Important information includes:

| Information | Purpose |
| :--- | :--- |
| **Object name** | Check the file name |
| **Object URL** | Reference the resource |
| **Size** | Check file size |
| **Last modified** | Check upload time |

---

## 4. Configure Access Permissions

In the CyberNet system, files stored on S3 should have proper access control.

Some notes:

- Do not make sensitive files public.
- Only allow access to files that need to be displayed to users.
- Do not upload Access Keys, Secret Keys, or `.env` files to S3.
- CloudFront can be used to distribute content from S3.

---

## 5. Connect S3 Data with Backend

The backend can store S3 file paths or URLs as part of product data.

Example product data:

```json
{
  "name": "Mixed noodles",
  "price": 15000,
  "stock": 10,
  "imageKey": "products/product-1.png",
  "imageUrl": "https://example-cloudfront-domain/products/product-1.png"
}
```

When the Frontend displays products, the system uses `imageUrl` to load images from S3 or CloudFront.

---

## Result

After completing this section, the CyberNet system can:

- Store product images on Amazon S3.
- Manage static files and uploaded resources.
- Organize objects by folders.
- Use S3 or CloudFront URLs in product data.
- Prepare data for connection with Backend and Frontend.
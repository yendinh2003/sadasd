# Proposal-FJC
# 🚀 Upload Image to S3 bằng API Gateway và Lambda

📌 **Người thực hiện:** Vũ Yên Định  
🆔 **MSSV:** [2180602169]  
🏫 **Trường:** [HUTECH]  
📅 **Ngày thực hiện:** 07/07/2025

---

## 🧭 Table of Contents

1. [📄 Executive Summary](#-executive-summary)  
2. [🔍 1. Problem Statement](#-1-problem-statement)  
3. [🛠 2. Solution Architecture](#-2-solution-architecture)  
4. [🧪 3. Technical Implementation](#-3-technical-implementation)  
5. [📆 4. Timeline & Milestones](#-4-timeline--milestones)  
6. [💰 5. Budget Estimation](#-5-budget-estimation)  
7. [⚠️ 6. Risk Assessment](#-6-risk-assessment)  
8. [🎯 7. Expected Outcomes](#-7-expected-outcomes)  
9. [📎 Appendices](#-appendices)

---

## 📄 Executive Summary

> Giải pháp **serverless** giúp tải ảnh lên **Amazon S3** thông qua **API Gateway** và **Lambda**.  
> ✅ Loại bỏ backend truyền thống  
> ✅ Tiết kiệm chi phí  
> ✅ Mở rộng linh hoạt  
> ✅ Phù hợp với app web/mobile có chức năng upload ảnh.

---

## 🔍 1. Problem Statement

### 🧩 1.1 Current Situation
- Ứng dụng thường yêu cầu backend để xử lý ảnh.
- Backend tốn chi phí, bảo trì khó khăn, dễ phát sinh lỗi.

### ❗ 1.2 Key Challenges
- Chi phí hạ tầng tăng cao.
- Không linh hoạt nếu user tăng nhanh.
- Bảo mật file ảnh phức tạp.
- Developer tốn thời gian.

### 👥 1.3 Stakeholder Impact
| Đối tượng        |   Ảnh hưởng                              |  
|------------------|------------------------------------------|
| Developer        | Tốn thời gian viết backend               |
| Doanh nghiệp     | Tăng chi phí & thời gian                 |

### 💥 1.4 Business Consequences
- Giảm tốc độ triển khai sản phẩm.
- Gia tăng lỗi & rủi ro bảo mật.
- Trải nghiệm người dùng kém hơn.

---

## 🛠 2. Solution Architecture

### 🧱 2.1 Architecture Overview

Client → API Gateway → Lambda → Amazon S3


### ☁️ 2.2 AWS Services Used

| 🧩 Dịch vụ        | 🎯 Vai trò chính                         |
|------------------|------------------------------------------|
| Amazon S3        | Lưu trữ ảnh                             |
| AWS Lambda       | Xử lý dữ liệu ảnh                       |
| API Gateway      | Entry point nhận request                |
| IAM Role         | Phân quyền cho Lambda                   |
| CloudWatch (*)   | Ghi log, theo dõi hoạt động Lambda      |

### 🧬 2.3 Component Design
1. Client gửi ảnh qua POST.
2. API Gateway nhận và gọi Lambda.
3. Lambda xử lý ảnh và upload lên S3.
4. Ảnh được lưu trong S3 theo timestamp.

### 🔐 2.4 Security Architecture
- **IAM Role riêng** cho Lambda chỉ được `s3:PutObject`.
- **Block All Public Access** trên S3.
- Có thể dùng API Key hoặc JWT để bảo vệ API Gateway.

### 📈 2.5 Scalability Design
- Lambda & API Gateway tự mở rộng theo lưu lượng.
- Amazon S3 lưu trữ không giới hạn.
- Không cần quản lý server.

---

## 🧪 3. Technical Implementation

### 🔄 3.1 Implementation Phases

| Giai đoạn   | Công việc chính                           |
|-------------|--------------------------------------------|
| Giai đoạn 1 | Tạo Bucket + IAM Role                      |
| Giai đoạn 2 | Viết Lambda function                       |
| Giai đoạn 3 | Cấu hình API Gateway                       |
| Giai đoạn 4 | Kiểm thử với Postman                       |
| Giai đoạn 5 | Viết báo cáo, tạo sơ đồ, tổng kết          |

### 🧰 3.2 Technical Requirements
- Tài khoản AWS (Free Tier).
- Biết Node.js hoặc Python cơ bản.
- Sử dụng Postman để gửi ảnh (base64 hoặc multipart).

### 🏗 3.3 Development Approach
- Viết code rõ ràng, chia module nhỏ.
- Log chi tiết bằng `console.log()` trong Lambda.
- Biến cấu hình tách riêng bằng environment variable.

### 🧪 3.4 Testing Strategy
- Upload ảnh đúng định dạng.
- Test lỗi dung lượng, sai định dạng.
- Debug bằng CloudWatch Logs.

### 🚀 3.5 Deployment Plan
- Deploy thủ công trên AWS Console.
- Có thể mở rộng dùng CloudFormation, SAM, Terraform.

---

## 📆 4. Timeline & Milestones

| Thời gian | Mốc hoàn thành                              |
|-----------|----------------------------------------------|
| Tuần 1    | Tạo bucket, IAM role, viết Lambda            |
| Tuần 2    | Cấu hình API Gateway, test thành công        |
| Tuần 3    | Viết báo cáo, vẽ sơ đồ, tổng hợp nộp         |

---

## 💰 5. Budget Estimation

| Dịch vụ       | Dự kiến chi phí (Free Tier)               |
|---------------|-------------------------------------------|
| S3            | ~$0.023/GB (Miễn phí 5GB đầu)             |
| Lambda        | Miễn phí dưới 1 triệu request/tháng       |
| API Gateway   | Miễn phí 1 triệu request đầu              |
| **Tổng cộng** | 🟢 Gần như 0 đồng trong giới hạn Free Tier |

---

## ⚠️ 6. Risk Assessment

| 🧨 Rủi ro              | Hậu quả                      | Cách giảm thiểu                         |
|------------------------|------------------------------|------------------------------------------|
| IAM sai                | Lambda không upload được     | Gán đúng quyền `s3:PutObject`            |
| File lớn quá           | Timeout                      | Giới hạn dung lượng client (<= 5MB)      |
| Cấu hình API sai       | Không nhận ảnh               | Kiểm thử kỹ bằng Postman + CloudWatch    |

---

## 🎯 7. Expected Outcomes

### ✅ Success Metrics
- Upload ảnh thành công ≥ 95%.
- Xử lý dưới 500ms.
- Ảnh lưu đúng vị trí, không bị công khai.

### 📈 Business Benefits
- Nhanh chóng thêm tính năng upload vào hệ thống.
- Tiết kiệm nhân lực backend.
- Tăng trải nghiệm người dùng.

### 🔧 Technical Gains
- Làm chủ AWS Serverless cơ bản.
- Biết tích hợp API Gateway + Lambda + S3.

---

## 📎 Appendices

### 📄 A. Technical Specifications

- Runtime: Node.js 18.x / Python 3.9
- API: HTTP POST /upload
- Input: base64 hoặc multipart/form-data
- Output: ảnh lưu tại `s3://bucket-name/uploads/filename.jpg`

### 💸 B. Cost Calculations

- 1GB ảnh → ~$0.023 USD
- 1000 request Lambda/Gateway → miễn phí
- Sử dụng trong Free Tier là hợp lý

### 🗺 C. Architecture Diagram

```text
+---------+        +-------------+        +-----------+        +-----------+
| Client  | POST → | API Gateway | → → →  |  Lambda   | → → →  |    S3     |
+---------+        +-------------+        +-----------+        +-----------+
```

### 🔗 D. References

- [📘 AWS Lambda Docs](https://docs.aws.amazon.com/lambda/)
- [🌐 API Gateway Docs](https://docs.aws.amazon.com/apigateway/)
- [🗂️ Amazon S3 Docs](https://docs.aws.amazon.com/s3/)



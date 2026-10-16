---
name: aws-deployment
description: >-
  Provides procedures and runbooks for deploying the ToDo-List fullstack application to Amazon Web Services (AWS).
  Activate this skill when the user asks to deploy, configure AWS infrastructure, build Docker containers,
  or set up CI/CD for Frontend (S3/CloudFront) and Backend (App Runner/ECS) with MongoDB Atlas.
---

# AWS Deployment Runbook for ToDo-List

Hướng dẫn từng bước triển khai ứng dụng ToDo List lên hạ tầng Amazon Web Services (AWS).

## 1. Cơ Sở Dữ Liệu: MongoDB Atlas on AWS
1. Đăng ký/Đăng nhập [MongoDB Atlas](https://www.mongodb.com/atlas).
2. Tạo cụm **M0 Free Tier (512MB)**:
   - Cloud Provider: **Amazon Web Services (AWS)**.
   - Region: Chọn khu vực gần người dùng nhất (ví dụ: `ap-southeast-1` Singapore).
3. Thiết lập **Database Access**: Tạo người dùng database (username & password).
4. Thiết lập **Network Access**:
   - Trong quá trình test: Thêm IP `0.0.0.0/0` (Allow Access from Anywhere).
   - Production: Cấu hình dải IP của AWS App Runner / NAT Gateway hoặc AWS VPC Peering.
5. Lấy chuỗi kết nối (`mongodb+srv://...`) để điền vào biến môi trường `MONGODB_URI` của Backend.

---

## 2. Backend: NestJS Container trên AWS App Runner
App Runner là giải pháp tối ưu và tiết kiệm nhất cho NestJS trên AWS:

1. **Tạo Dockerfile cho Backend**:
   - Base image: `node:22-alpine`.
   - Build ứng dụng: `npm run build`.
   - Expose port: `4000`.
2. **Đẩy Docker Image lên AWS ECR**:
   ```bash
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com
   docker build -t todo-backend ./backend
   docker tag todo-backend:latest <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com/todo-backend:latest
   docker push <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com/todo-backend:latest
   ```
3. **Cấu hình Service trên AWS App Runner**:
   - Source: Chọn image từ ECR vừa tạo.
   - Environment variables:
     - `PORT=4000`
     - `MONGODB_URI=<chuỗi kết nối MongoDB Atlas>`
   - App Runner sẽ tự động cấp phát domain HTTPS và quản lý auto-scaling.

---

## 3. Frontend: React Vite trên AWS S3 + CloudFront
1. **Build tĩnh Frontend**:
   ```bash
   cd frontend
   npm run build
   ```
2. **Tạo S3 Bucket**:
   - Tên bucket: ví dụ `taskflow-todo-frontend`.
   - Bật tính năng Static website hosting (hoặc dùng CloudFront OAC).
3. **Đồng bộ mã build lên S3**:
   ```bash
   aws s3 sync ./dist s3://taskflow-todo-frontend --delete
   ```
4. **Tạo CloudFront Distribution**:
   - Origin: Trỏ tới S3 bucket.
   - Viewer Protocol Policy: `Redirect HTTP to HTTPS`.
   - Error Pages: Cấu hình `404` và `403` chuyển hướng về `/index.html` (mã HTTP 200) cho React Single Page Application (SPA).

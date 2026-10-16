# 📋 Kiến Trúc Hệ Thống & Đề Xuất Công Nghệ Cho Dự Án ToDo-List (MongoDB + AWS)

Tài liệu này trình bày tổng quan kiến trúc, đề xuất triển khai cơ sở dữ liệu **MongoDB** trên **Amazon Web Services (AWS)**, mô hình triển khai và lộ trình phát triển chi tiết cho hệ thống ToDo List.

---

## 1. 🎯 Tổng Quan Hệ Thống

- **Frontend (FE)**: React + TypeScript (Vite, Responsive, Modern Glassmorphism/Neumorphic UI, Micro-animations, Type-safe).
- **Backend (BE)**: NestJS + TypeScript (Modular Architecture, `@nestjs/mongoose` + Mongoose Schema, Class-validator, Swagger OpenAPI `/api/docs`).
- **Database (DB)**: **MongoDB** (Linh hoạt, NoSQL Document-based, tốc độ cao, hỗ trợ lưu trữ JSON trực quan).
- **Cấu trúc Repository**:
  - `frontend/`: Một Git repository độc lập (`git init`).
  - `backend/`: Một Git repository độc lập (`git init`).
- **Triết lý thiết kế ban đầu**: Đơn giản, tinh gọn, có thể chạy local ngay với MongoDB hoặc Docker MongoDB, đồng thời cắm nối ngay lập tức lên **MongoDB Atlas on AWS** hoặc **AWS DocumentDB** chỉ bằng 1 biến môi trường `MONGODB_URI`.

---

## 2. 🗄️ Triển Khai MongoDB Trên AWS: Nên Dùng Dịch Vụ Nào?

Khi triển khai MongoDB trên AWS, bạn có 3 phương án chính:

### 🏆 Phương Án Tối Ưu Nhất: **MongoDB Atlas hosted on AWS** (Khuyên Dùng ⭐⭐⭐⭐⭐)

| Tiêu chí | Đánh giá |
| :--- | :--- |
| **Bản chất** | Dịch vụ Managed Database chính chủ của MongoDB, chạy trực tiếp trên hạ tầng cụm máy chủ AWS (ví dụ: `ap-southeast-1` Singapore). |
| **Gói Miễn Phí (Free Tier)** | **M0 Sandbox Free vĩnh viễn (512MB RAM & Storage)** - Quá đủ cho giai đoạn phát triển, thử nghiệm và đồ án/ứng dụng cá nhân. Không tốn 1 xu. |
| **Tích hợp AWS** | Hỗ trợ VPC Peering / AWS PrivateLink để kết nối an toàn với Backend NestJS chạy trên AWS (EC2/ECS/App Runner). |
| **Bảo trì & Sao lưu** | Tự động backup, tự động scale, có dashboard theo dõi query, chỉ mục (Index) trực quan. |

> 👉 **Kết luận**: Đây là lựa chọn **số 1** cho hầu hết các startup và dự án NestJS dùng MongoDB vì miễn phí khởi đầu, thiết lập chỉ mất 3 phút, hiệu năng cực cao khi chọn cùng AWS Region với server backend.

---

### Các Phương Án Khác Trên AWS (So Sánh):

| Giải pháp | Ưu điểm | Nhược điểm / Chi phí | Khi nào nên dùng? |
| :--- | :--- | :--- | :--- |
| **Amazon DocumentDB** *(AWS Native)* | • Tích hợp sâu vào hệ sinh thái AWS (IAM, CloudWatch, KMS).<br>• Tương thích chuẩn API MongoDB 4.0/5.0. | • **Chi phí tối thiểu khá cao**: Instance nhỏ nhất (`db.t3.medium`) khoảng **~$55 - $70/tháng** (chỉ có trial 30 ngày, không có gói Free vĩnh viễn). | Dành cho doanh nghiệp lớn bắt buộc toàn bộ dữ liệu phải nằm 100% trong VPC của AWS do tuân thủ bảo mật compliance nghiêm ngặt. |
| **Tự host MongoDB trên AWS EC2** *(Docker)* | • Kiểm soát 100% cấu hình, tài nguyên.<br>• Tận dụng được EC2 Free Tier (`t4g.small` hoặc `t3.micro`). | • Bạn phải tự quản lý sao lưu (backup), tự bảo mật tường lửa, cập nhật bản vá hệ điều hành. | Khi muốn tiết kiệm chi phí gom chung Backend + DB vào cùng 1 con EC2 duy nhất qua `docker-compose`. |

---

## 3. 🚀 Mô Hình Triển Khai Lên AWS (AWS Deployment Architecture)

```
[ Người Dùng (Browser / Mobile) ]
       │
       ├──────────────────────────────────────────────┐
       ▼ (Tải UI Frontend)                            ▼ (Gọi API REST / Swagger)
[ AWS CloudFront (CDN + SSL miễn phí) ]      [ AWS App Runner / ECS Fargate / EC2 ]
       │                                              │  (Chạy NestJS Backend)
       ▼                                              │
[ AWS S3 Bucket (Chứa FE React Build) ]               │ (Kết nối qua MONGODB_URI)
                                                      ▼
                                       ┌───────────────────────────────┐
                                       │    MongoDB Atlas on AWS       │
                                       │   (Region: ap-southeast-1)    │
                                       └───────────────────────────────┘
```

### Hướng dẫn kết nối Backend với MongoDB:
Backend NestJS chỉ cần đọc biến môi trường trong file `.env`:
```env
# Local development (Chạy máy tính cá nhân hoặc Docker)
MONGODB_URI=mongodb://localhost:27017/todo-db

# Hoặc kết nối trực tiếp MongoDB Atlas trên AWS:
# MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.abcde.mongodb.net/todo-db?retryWrites=true&w=majority
PORT=4000
```

---

## 4. 📁 Cấu Trúc Mã Nguồn (Repo Structure)

Dự án gồm 2 Git repository tách biệt độc lập:

```text
ToDo-List/
├── ARCHITECTURE_AND_RECOMMENDATIONS.md    <-- Tài liệu này
├── docker-compose.yml                     <-- File chạy nhanh MongoDB cho môi trường Local (nếu cần)
│
├── frontend/                               <-- [GIT REPO 1]
│   ├── .git/
│   ├── src/
│   │   ├── components/                    <-- Header, TodoInput, TodoItem, FilterTabs, StatsBar
│   │   ├── services/                      <-- api.ts (Axios kết nối NestJS Backend)
│   │   ├── types/                         <-- todo.interface.ts
│   │   ├── App.tsx                        <-- Todo Application UI chính
│   │   ├── App.css                        <-- Theme, Glassmorphism, animations
│   │   └── main.tsx
│   ├── package.json
│   └── vite.config.ts
│
└── backend/                                <-- [GIT REPO 2]
    ├── .git/
    ├── src/
    │   ├── todos/
    │   │   ├── dto/                       <-- create-todo.dto.ts, update-todo.dto.ts
    │   │   ├── schemas/                   <-- todo.schema.ts (@Schema, @Prop từ Mongoose)
    │   │   ├── todos.controller.ts        <-- CRUD Endpoints (@Get, @Post, @Patch, @Delete)
    │   │   ├── todos.service.ts           <-- Logic thao tác với Model Todo
    │   │   └── todos.module.ts
    │   ├── app.module.ts                  <-- MongooseModule.forRootAsync(...)
    │   └── main.ts                        <-- Enable CORS, Swagger Docs (/api/docs)
    ├── .env.example
    └── package.json
```

---

## 5. 🗺️ Lộ Trình Triển Khai (Roadmap)

1. **Hiện tại (Cốt lõi - Đơn giản & Hoàn chỉnh)**:
   - Backend NestJS với Mongoose, schema Todo (title, description, isCompleted, priority, createdAt).
   - Frontend React TS giao diện hiện đại, UX mượt mà, đầy đủ tính năng: thêm, sửa, xóa, tìm kiếm, lọc theo trạng thái, đánh dấu ưu tiên.
   - Swagger documentation tự động tại `http://localhost:4000/api/docs`.
   - Cung cấp sẵn `docker-compose.yml` để bạn có thể gõ `docker compose up -d` là có ngay MongoDB chạy local nếu máy chưa cài MongoDB.
2. **Giai đoạn tiếp theo**:
   - Thêm tính năng đăng nhập / phân quyền (Authentication với JWT + Passport).
   - Đưa Backend lên AWS App Runner hoặc ECS.
   - Tạo cụm MongoDB Atlas Free M0 trên AWS region Singapore (`ap-southeast-1`) và dán connection string vào biến môi trường.
   - Build FE đẩy lên AWS S3 + CloudFront.

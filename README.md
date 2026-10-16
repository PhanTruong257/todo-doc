# 🚀 TaskFlow - Fullstack ToDo List System

Hệ thống quản lý công việc ToDo List hoàn chỉnh được xây dựng theo kiến trúc chuẩn doanh nghiệp:
- **Frontend**: React 19 + TypeScript + Vite (Giao diện Glassmorphism hiện đại, responsive, micro-animations, lọc & tìm kiếm thông minh).
- **Backend**: NestJS 12 + TypeScript (Kiến trúc module hóa, Mongoose ODM, Validation DTO, tích hợp tài liệu tương tác Swagger OpenAPI).
- **Database**: **MongoDB** (hỗ trợ chạy cục bộ hoặc kết nối đám mây **MongoDB Atlas trên AWS** / AWS DocumentDB).
- **Git Structure**: `frontend` và `backend` là **2 Git repository độc lập**.

---

## 📑 Tài Liệu Dự Án (Thư mục docs/)

- 📐 **Kiến trúc & Đề xuất AWS**: [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) (So sánh DB trên AWS, mô hình triển khai S3/CloudFront + App Runner).
- 📋 **Tiến độ & Danh sách tính năng**: [docs/FEATURES.md](./docs/FEATURES.md) (Checklist chi tiết tính năng đã hoàn thiện và roadmap).
- 🚀 **Kế hoạch triển khai S3 + CloudFront**: [docs/DEPLOY_S3_PLAN.md](./docs/DEPLOY_S3_PLAN.md) (Hướng dẫn từng bước đưa Frontend lên AWS S3 và CloudFront CDN).

---

## ⚡ Hướng Dẫn Chạy Nhanh (Quick Start)

### Bước 1: Khởi động MongoDB
Bạn có 2 lựa chọn:

- **Cách 1 (Khuyên dùng khi dev local)**: Dùng Docker Compose có sẵn tại thư mục gốc:
  ```bash
  docker compose up -d
  ```
- **Cách 2**: Dùng cụm miễn phí **MongoDB Atlas** trên AWS (hoặc MongoDB cài trên máy):
  Mở file `backend/.env` và cập nhật `MONGODB_URI`:
  ```env
  MONGODB_URI=mongodb+srv://<user>:<password>@cluster0.abc.mongodb.net/todo-db
  ```

---

### Bước 2: Chạy Backend NestJS
Mở một terminal mới:
```bash
cd backend
npm run start:dev
```
- **REST API Server**: `http://localhost:4000`
- **Swagger API Docs tương tác**: `http://localhost:4000/api/docs`

---

### Bước 3: Chạy Frontend React TS
Mở một terminal khác:
```bash
cd frontend
npm run dev
```
- Mở trình duyệt tại: `http://localhost:5173`
- *Lưu ý: Nếu chưa bật Backend, giao diện Frontend vẫn hoạt động trơn tru với chế độ Local Preview mượt mà!*

---

## 📁 Cấu Trúc Dự Án & Quản Lý Git

Mỗi thư mục con là một kho lưu trữ Git riêng:

| Thư mục | Công nghệ | Git Repository | Chức năng |
| :--- | :--- | :--- | :--- |
| `frontend/` | React, TypeScript, Vite, CSS Modules | Độc lập (`frontend/.git`) | Giao diện người dùng, kết nối Axios API, bộ lọc, thống kê |
| `backend/` | NestJS, TypeScript, Mongoose, Swagger | Độc lập (`backend/.git`) | REST API CRUD, validation, kết nối MongoDB |
| `docker-compose.yml` | Docker, MongoDB 7.0 | - | Khởi động MongoDB local trong 5 giây |

---

## 🛠️ Danh Sách API Endpoints (NestJS)

| Phương thức | Đường dẫn | Chức năng |
| :--- | :--- | :--- |
| `POST` | `/todos` | Tạo mới một công việc |
| `GET` | `/todos` | Lấy danh sách công việc (hỗ trợ `search`, `status`, `priority`) |
| `GET` | `/todos/stats` | Lấy số liệu thống kê (tổng, đang làm, đã xong, ưu tiên cao) |
| `GET` | `/todos/:id` | Xem chi tiết 1 công việc theo ObjectId |
| `PATCH` | `/todos/:id` | Cập nhật thông tin công việc |
| `PATCH` | `/todos/:id/toggle` | Đảo trạng thái hoàn thành (Toggle complete) |
| `DELETE` | `/todos/:id` | Xóa công việc |

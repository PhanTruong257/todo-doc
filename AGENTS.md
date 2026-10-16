# 🤖 Hướng Dẫn Dành Cho Antigravity Agent (AGENTS.md)

Chào mừng bạn đến với dự án **TaskFlow (ToDo-List Fullstack)**. Đây là file chỉ dẫn cho Antigravity AI Agent khi hỗ trợ lập trình trong workspace này.

---

## 📌 Tổng Quan Kiến Trúc
- **Root Repo (`./`)**: Quản lý tài liệu dự án, cấu hình AI Agent (`.agents/`, `docs/`, `AGENTS.md`). Bỏ qua `source/` trong `.gitignore`.
- **Frontend (`source/frontend/`)**: React 19 + TypeScript + Vite. Độc lập với Git repo riêng (`todo-fe`).
- **Backend (`source/backend/`)**: NestJS 12 + TypeScript + Mongoose. Độc lập với Git repo riêng (`todo-be`).
- **Multi-Root Workspace**: File `taskflow.code-workspace` hiển thị đồng thời cả 3 git repositories trên IDE.
- **Database**: MongoDB (hỗ trợ Docker cục bộ và MongoDB Atlas trên AWS).
- **Hạ tầng triển khai AWS**:
  - FE: AWS S3 + CloudFront (CDN + SSL) - Live tại `https://d3joa566sl3ve2.cloudfront.net`.
  - BE: AWS EC2 Ubuntu 24.04 Docker (`t3.micro`, IP: `13.228.71.202`).
  - DB: MongoDB Atlas (Cụm AWS Singapore `ap-southeast-1`, gói Free M0 512MB).
  - Reverse Proxy: CloudFront định tuyến `/*` sang S3 và `/api/*` sang EC2.
  - Xem chi tiết toàn bộ tài liệu hạ tầng & runbook tại [docs/INFRASTRUCTURE.md](./docs/INFRASTRUCTURE.md).

---

## ⚡ Các Lệnh Thường Dùng Khi Hỗ Trợ Người Dùng

### Khởi động Database:
```bash
docker compose up -d
```

### Chạy Backend NestJS:
```bash
cd source/backend
npm run start:dev
# Kiểm tra Swagger Docs tại: http://localhost:4000/api/docs
```

### Chạy Frontend React TS:
```bash
cd source/frontend
npm run dev
# Mở Web tại: http://localhost:5173
```

---

## 🎯 Quy Tắc Cốt Lõi Khi Code
1. **Kiểm tra tiến độ tính năng (BẮT BUỘC)**:
   - Trước khi phát triển hoặc sửa đổi bất kỳ tính năng nào, Agent **BẮT BUỘC ĐỌC FILE [docs/FEATURES.md](./docs/FEATURES.md)** để nắm rõ những gì đã làm, tránh viết code trùng lặp hoặc phá vỡ tính năng có sẵn.
   - Sau khi hoàn thành xong tính năng, Agent **BẮT BUỘC CẬP NHẬT LẠI FILE [docs/FEATURES.md](./docs/FEATURES.md)** (đánh dấu hoàn thành và ghi nhật ký thay đổi).
2. **Quy chuẩn Co-location Component (Frontend)**:
   - Mỗi component nằm trong thư mục riêng: `Component.tsx` (view), `useComponent.ts` (hook tách logic), `Component.css` (styles). Không phân tán logic/style sang các folder khác.
3. **Bảo tồn tính độc lập Git**: 3 Git repo độc lập hoàn toàn (Root repo quản lý docs/agents và ignore `source/`; `source/frontend/` và `source/backend/` có Git repo riêng).
4. **Type Safety**: Luôn giữ chuẩn TypeScript nghiêm ngặt (`verbatimModuleSyntax`, `import type`).
5. **Mongoose DTO & Validation**: Mọi endpoint mới trong NestJS phải có đầy đủ DTO validator và Swagger decorators (`@ApiTags`, `@ApiOperation`).
6. **Chỉ Push/Deploy khi người dùng yêu cầu & BẮT BUỘC Check Build trước (Local First)**:
   - Mọi tính năng, sửa đổi UI/Logic phải được kiểm thử và chạy mượt mà ở môi trường Local trước (`npm run dev` tại `http://localhost:5173`).
   - **Bắt buộc Check Build (`npm run build`)**: Trước khi thực hiện bất kỳ lệnh `git commit`, `git push` hoặc tạo `git tag` nào, Agent **BẮT BUỘC PHẢI CHẠY LỆNH BUILD** (Frontend: `cd source/frontend && npm run build`, Backend: `cd source/backend && npm run build`) để xác nhận biên dịch sạch sẽ, đạt **0 lỗi TypeScript / syntax**. Tuyệt đối không bao giờ push code bị lỗi build lên GitHub gây sập CI/CD pipeline.
   - Agent **TUYỆT ĐỐI KHÔNG** tự ý chạy `git push` hoặc tạo `git tag` lên GitHub nếu người dùng chưa ra lệnh rõ ràng ("push đi", "deploy đi", "tạo tag đi").
7. **Đáp ứng hoàn hảo giao diện điện thoại (Mobile Responsive - BẮT BUỘC & TIÊN QUYẾT)**:
   - **Tất cả các tính năng và thành phần UI** (Header, Dropdown menu, Sidebar, DatePicker, Thẻ bài học, Form nhập liệu, Thống kê, Bảng chữ cái, Bàn phím ảo, Modal...) **BẮT BUỘC PHẢI RESPONSIVE 100% TRÊN MOBILE** (< 1024px, < 768px, < 480px, và < 360px).
   - **Quy tắc Zero-Horizontal-Scroll**: Tuyệt đối không để xảy ra hiện tượng tràn màn hình ngang (horizontal scrollbar), vỡ khung layout hoặc trôi mép trên điện thoại.
   - **Tối ưu thao tác chạm (Touch-Friendly)**: Mọi nút bấm, tab chọn, ô input và item danh sách phải có chiều cao tối thiểu 36px - 44px, dễ dàng bấm bằng một ngón tay.
   - **Popup & Dropdown an toàn**: Mọi menu dropdown, popover lịch hay modal phải tự co giãn vừa vặn với viewport di động, không bị che khuất hoặc cắt cụt mép màn hình.
8. **Tham khảo Skills**:
   - Khi cần triển khai hoặc cấu hình AWS: Kích hoạt skill [aws-deployment](file:///d:/Code/ToDo-List/.agents/skills/aws-deployment/SKILL.md).
   - Xem thêm quy tắc chi tiết tại [project-rules.md](file:///d:/Code/ToDo-List/.agents/rules/project-rules.md).


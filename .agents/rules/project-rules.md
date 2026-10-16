# Quy Tắc Phát Triển Dự Án ToDo-List (Antigravity Rules)

Tài liệu này quy định các tiêu chuẩn và nguyên tắc mà Antigravity Agent cần tuân thủ khi làm việc trên repository ToDo-List.

## 1. Cấu Trúc Độc Lập Giữa Frontend và Backend
- Thư mục `source/frontend/` và `source/backend/` là **2 Git repository độc lập**. Thư mục root cha quản lý docs/agents và bỏ qua `source/` trong `.gitignore`.
- Tuyệt đối không can thiệp chéo làm hỏng `.git` riêng của mỗi thư mục mã nguồn.
- Mọi commit hoặc lệnh Git phải được thực hiện trong đúng thư mục tương ứng (`cd source/frontend` hoặc `cd source/backend`).

## 2. Tiêu Chuẩn Frontend (React + TypeScript)
- **Công nghệ**: React, TypeScript, Vite.
- **Styling & Cấu trúc Component**:
  - Áp dụng triệt để mô hình **Co-location**: Mỗi component sở hữu riêng 1 thư mục, bao gồm `Component.tsx` (view), `useComponent.ts` (hook tách logic) và `Component.css` (style riêng) nằm chung 1 chỗ, không tách rời lung tung.
  - Sử dụng Vanilla CSS với biến màu định nghĩa sẵn tại `src/index.css`. Tránh tự ý cài đặt TailwindCSS.
  - Tối ưu thẩm mỹ (Modern Dark/Light Glassmorphism, Micro-animations, responsive trên mobile/desktop).
- **TypeScript**: Tuân thủ nghiêm ngặt `verbatimModuleSyntax`, luôn sử dụng `import type` khi import interface/type.

## 3. Tiêu Chuẩn Backend (NestJS + TypeScript)
- **Kiến trúc**: Tuân thủ chuẩn Modular Architecture của NestJS (`Module`, `Controller`, `Service`, `DTO`, `Schema`).
- **Database**:
  - Cơ sở dữ liệu chính là **MongoDB** (thông qua `@nestjs/mongoose` và Mongoose Schema).
  - Cấu hình kết nối linh hoạt thông qua biến môi trường `MONGODB_URI` trong file `.env`.
  - Luôn hỗ trợ `_id` chuyển đổi thành `id` dạng string khi trả về JSON cho client.
- **Validation & API Docs**:
  - Dùng `class-validator` và `class-transformer` cho toàn bộ DTO request.
  - Luôn cung cấp tài liệu Swagger OpenAPI (`@ApiTags`, `@ApiOperation`, `@ApiResponse`) tại đường dẫn `/api/docs`.
- **CORS**: Luôn bật CORS trong `main.ts` để Frontend gọi được API.

## 4. Quản Lý Biến Môi Trường
- Luôn duy trì file `.env.example` đồng bộ với bất kỳ biến môi trường mới nào được thêm vào code.
- Không commit các file chứa secret hoặc thông tin nhạy cảm (`.env`) lên Git.

## 5. Quản Lý Tiến Độ & Tính Năng (docs/FEATURES.md)
- **Đọc trước khi làm**: Trước khi phát triển bất kỳ tính năng mới nào, Agent bắt buộc đọc file `docs/FEATURES.md` để nắm danh sách tính năng đã hoàn thiện và quy ước hiện tại.
- **Cập nhật sau khi xong**: Sau khi hoàn thành hoặc sửa đổi tính năng, Agent bắt buộc cập nhật lại checklist và ghi nhật ký thay đổi (Changelog) trong `docs/FEATURES.md`.

## 6. Quy Tắc Test Local & Kiểm Soát Push/Deploy (Local First & Check Build - BẮT BUỘC)
- **Local First**: Mọi tính năng, sửa đổi giao diện (UI) hoặc logic phải được phát triển và kiểm thử chạy mượt mà ở môi trường Local trước (`npm run dev` trên localhost).
- **Bắt buộc Check Build (`npm run build`)**: Trước khi thực hiện bất kỳ thao tác `git commit`, `git push` hoặc tạo `git tag` nào, Agent **BẮT BUỘC PHẢI CHẠY LỆNH BUILD** (`cd source/frontend && npm run build` hoặc `cd source/backend && npm run build`) để kiểm tra nghiêm ngặt kiểu dữ liệu TypeScript (`tsc -b`) và xác nhận biên dịch production bundle đạt **0 lỗi**. Tuyệt đối không bao giờ push code bị gãy build lên GitHub gây sập CI/CD GitHub Actions.
- **Tuyệt đối KHÔNG tự ý Push/Tag**: Agent **NGHIÊM CẤM** tự ý chạy `git push` hoặc tạo `git tag` lên GitHub nếu người dùng chưa yêu cầu rõ ràng (ví dụ: "push đi", "hãy deploy", "tạo tag lên AWS đi").
- Luôn hướng dẫn hoặc để người dùng trải nghiệm, test thử trên Local UI trước. Chỉ khi người dùng đồng ý và ra lệnh push thì Agent mới được thực hiện lệnh push / tag.

## 7. Tiêu Chuẩn Giao Diện Điện Thoại (Mobile Responsive - BẮT BUỘC & TIÊN QUYẾT)
- **Thiết kế Mobile-First**: Mọi màn hình và thành phần giao diện (Header, Dropdown menu, Thanh điều hướng giai đoạn, Bảng chữ cái Hangeul, Thẻ từ vựng, Ngữ pháp, Hội thoại, Bàn phím ảo IME, Modal...) PHẢI hiển thị hoàn mỹ trên mọi kích cỡ màn hình di động (< 1024px, < 768px, < 480px, và < 360px).
- **Không tràn màn hình (Zero Horizontal Scroll)**: Đảm bảo không có bất kỳ thành phần nào làm xuất hiện thanh cuộn ngang ngoài ý muốn trên toàn trang (`overflow-x: hidden`).
- **Thao tác chạm thuận tiện (Touch-friendly)**: Mọi nút bấm, tab chọn, dropdown item, thẻ học phải có kích thước tối thiểu từ 36px - 44px và khoảng cách hợp lý để người dùng thao tác dễ dàng bằng một ngón tay cái.
- **Dropdown & Modal co giãn thông minh**: Dropdown menu, popover hay modal khi mở ra phải căn chỉnh theo chiều rộng màn hình thiết bị, có bóng mờ đóng khi bấm ngoài hoặc nút thoát rõ ràng, không bao giờ bị cắt cụt mép ngoài viewport.



# 📋 Kế Hoạch Triển Khai Frontend Lên AWS S3 + CloudFront (Kèm CI/CD Tự Động Qua Git Tag)

Tài liệu này hướng dẫn chi tiết quy trình triển khai Frontend tự động thông qua **GitHub Actions CI/CD**.  
Mỗi khi bạn tạo một phiên bản mới (Git Tag, ví dụ: `v1.0.0`, `v1.0.1`), GitHub sẽ tự động:
1. Tải mã nguồn & cài đặt Node.js 22.
2. Chạy `npm run build` đóng gói Vite.
3. Đẩy thẳng thư mục `dist/` lên **AWS S3 Bucket**.
4. Xóa cache trên **AWS CloudFront** để người dùng lập tức nhận được giao diện mới.

---

## 🎯 1. Mô Hình Tự Động Hóa (CI/CD Pipeline)

```
[ Lập Trình Viên ] ─── (Tạo tag: git tag v1.0.0 && git push origin v1.0.0) ───► [ GitHub Actions ]
                                                                                      │
                                                                       ┌──────────────┴──────────────┐
                                                                       ▼                             ▼
                                                        [ Build Vite: npm run build ]    [ Kết nối AWS IAM Key ]
                                                                       │                             │
                                                                       └──────────────┬──────────────┘
                                                                                      ▼
                                                                     [ Đồng bộ lên AWS S3 Bucket ]
                                                                                      │
                                                                                      ▼
                                                                     [ Xóa cache CloudFront (CDN) ]
```

---

## ⚙️ 2. Các Bước Cài Đặt (Chỉ Cần Làm 1 Lần Duy Nhất)

### Bước 1: Tạo AWS Access Key (IAM) Cho GitHub Actions
Để GitHub Actions có quyền tải file lên S3 của bạn, cần tạo một cặp chìa khóa (Access Key):

1. Trên AWS Console, tìm kiếm dịch vụ **IAM**.
2. Nhìn vào menu bên trái, bấm **Users** (Người dùng).
3. Bấm nút cam **Create user**:
   - User name: `github-actions-deployer`.
   - Bấm **Next**.
4. **Set permissions (Cấp quyền)**:
   - Chọn **Attach policies directly**.
   - Tìm kiếm và tick chọn 2 quyền:
     - ✅ **`AmazonS3FullAccess`** (để upload file lên S3).
     - ✅ **`CloudFrontFullAccess`** (để xóa cache CloudFront khi có bản mới).
   - Bấm **Next** -> Bấm **Create user**.
5. **Lấy Chìa Khóa (Access Key)**:
   - Bấm vào tên người dùng vừa tạo (`github-actions-deployer`).
   - Chuyển sang tab **Security credentials**.
   - Kéo xuống mục **Access keys**, bấm **Create access key**.
   - Chọn mục đích: **Third-party service** (hoặc Other).
   - Tick xác nhận và bấm **Next** -> Bấm **Create access key**.
   - ⚠️ **LƯU LẠI 2 DÒNG NÀY**:
     - `Access key ID` (dạng `AKIA...`)
     - `Secret access key` (dạng chuỗi ký tự bí mật dài).

---

### Bước 2: Tạo S3 Bucket & CloudFront Trên AWS
*(Nếu bạn đã tạo S3 và CloudFront theo hướng dẫn trước thì bỏ qua bước này)*:

1. **Tạo S3 Bucket**: Tên ví dụ `taskflow-fe-phan` ở vùng **Singapore (`ap-southeast-1`)**.
2. **Tạo CloudFront Distribution**:
   - Origin trỏ vào S3 bucket vừa tạo.
   - Origin Access: Chọn **OAC** (Origin Access Control) và dán Bucket Policy vào S3.
   - Default root object: `index.html`.
   - Error pages: 403 và 404 trỏ về `/index.html` với mã `200`.

---

### Bước 3: Thêm Chìa Khóa Vào GitHub Secrets (Repo `todo-fe`)

1. Mở repo GitHub của bạn: 👉 **[https://github.com/PhanTruong257/todo-fe/settings/secrets/actions](https://github.com/PhanTruong257/todo-fe/settings/secrets/actions)**
2. Bấm nút xanh **`New repository secret`** để thêm 4 biến bảo mật:

| Tên Secret (Name) | Giá trị (Value) | Giải thích |
| :--- | :--- | :--- |
| **`AWS_ACCESS_KEY_ID`** | `AKIA...` *(từ Bước 1)* | Mã định danh truy cập AWS |
| **`AWS_SECRET_ACCESS_KEY`** | Chuỗi bí mật dài *(từ Bước 1)* | Mật khẩu truy cập AWS |
| **`AWS_S3_BUCKET`** | `taskflow-fe-phan` | Tên S3 Bucket bạn đã tạo |
| **`CLOUDFRONT_DISTRIBUTION_ID`**| `E1234567890ABC` | ID của CloudFront Distribution *(tùy chọn)* |
| **`VITE_API_URL`** | `http://13.228.71.202:4000` | Link API máy chủ AWS EC2 |

---

## 🚀 3. Cách Sử Dụng CI/CD (Mỗi Lần Muốn Deploy Bản Mới)

Workflow file đã được tạo sẵn tại [.github/workflows/deploy.yml](file:///d:/Code/ToDo-List/frontend/.github/workflows/deploy.yml).

Từ bây giờ, mỗi khi bạn code xong tính năng và muốn đưa lên bản sản phẩm, bạn chỉ cần gõ đúng **2 lệnh git**:

```bash
cd frontend

# 1. Tạo tag phiên bản mới
git tag v1.0.0

# 2. Đẩy tag lên GitHub
git push origin v1.0.0
```

Ngay lập tức:
* Bạn vào tab **Actions** trên GitHub: **[https://github.com/PhanTruong257/todo-fe/actions](https://github.com/PhanTruong257/todo-fe/actions)**
* Bạn sẽ thấy một tiến trình tự động chạy màu xanh lá:
  * 📥 Checkout code
  * ⚙️ Setup Node.js 22
  * 📦 Install dependencies
  * 🔨 Build Vite
  * 🚀 Deploy lên S3
  * 🔄 Invalidate CloudFront Cache
* Sau khoảng **45 giây**, toàn bộ trang web trên CloudFront sẽ tự động cập nhật phiên bản mới mà bạn không cần phải mở AWS Console lên upload thủ công!

---

## 🏷️ Quy Ước Đặt Tên Tag Phiên Bản:
* Bản đầu tiên: `v1.0.0`
* Sửa lỗi nhỏ / cập nhật giao diện nhẹ: `v1.0.1`, `v1.0.2`...
* Thêm tính năng lớn (ví dụ sau này thêm Auth): `v1.1.0`, `v2.0.0`...

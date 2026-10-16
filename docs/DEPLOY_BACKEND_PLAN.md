# 📋 Kế Hoạch CI/CD Tự Động Cho Backend (AWS EC2 + Docker + Git Tag)

Tài liệu này hướng dẫn chi tiết quy trình thiết lập **GitHub Actions CI/CD** cho Backend NestJS.  
Mỗi khi bạn tạo một phiên bản mới (Git Tag, ví dụ: `v1.0.0`, `v1.0.1`), GitHub Actions sẽ:
1. Tự động kết nối SSH vào máy chủ AWS EC2 (`13.228.71.202`).
2. Tải mã nguồn mới nhất theo đúng Git Tag.
3. Chạy `sudo docker build` tạo image mới.
4. Tự động tắt container cũ và khởi động container `todo-api` mới mà không làm gián đoạn hệ thống.

---

## 🎯 1. Mô Hình Tự Động Hóa Backend

```
[ Lập Trình Viên ] ─── (Tạo tag: git tag v1.0.0 && git push origin v1.0.0) ───► [ GitHub Actions ]
                                                                                      │
                                                                   (SSH An Toàn qua Port 22)
                                                                                      ▼
                                                                     [ AWS EC2 Máy Chủ Ubuntu ]
                                                                                      │
                                                                       ┌──────────────┴──────────────┐
                                                                       ▼                             ▼
                                                        [ git checkout tag v1.0.0 ]   [ docker build & restart ]
                                                                                                     │
                                                                                                     ▼
                                                                                       [ todo-api Online :4000 ]
```

---

## ⚙️ 2. Các Bước Cài Đặt (Chỉ Làm 1 Lần Duy Nhất)

### Bước 1: Tạo Chìa Khóa SSH Trên Máy Chủ EC2 Cho GitHub Actions
Mở cửa sổ terminal Ubuntu trên EC2 (bằng nút *Connect* trên web AWS), gõ khối lệnh sau để tạo chìa khóa SSH riêng cho GitHub:

```bash
# Tạo cặp chìa khóa SSH không cần mật khẩu
ssh-keygen -t rsa -b 4096 -f ~/.ssh/github_actions -N ""

# Thêm khóa công khai vào danh sách được phép truy cập
cat ~/.ssh/github_actions.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# In chìa khóa bí mật ra màn hình để copy
cat ~/.ssh/github_actions
```

Sau khi chạy lệnh `cat ~/.ssh/github_actions`, màn hình sẽ in ra một đoạn khóa bắt đầu bằng:  
`-----BEGIN OPENSSH PRIVATE KEY-----` và kết thúc bằng `-----END OPENSSH PRIVATE KEY-----`.  
👉 **Hãy copy toàn bộ đoạn mã này (bao gồm cả dòng đầu và dòng cuối)**.

---

### Bước 2: Thêm Secrets Vào GitHub Repo `todo-be`

1. Truy cập trang cài đặt Secrets của repo `todo-be`:  
   👉 **[https://github.com/PhanTruong257/todo-be/settings/secrets/actions](https://github.com/PhanTruong257/todo-be/settings/secrets/actions)**
2. Bấm nút xanh **`New repository secret`** để thêm 4 biến sau:

| Tên Secret (Name) | Giá trị (Value) | Giải thích |
| :--- | :--- | :--- |
| **`EC2_HOST`** | `13.228.71.202` | Địa chỉ Public IP của máy chủ EC2 |
| **`EC2_USER`** | `ubuntu` | Tên người dùng mặc định trên Ubuntu EC2 |
| **`EC2_SSH_KEY`** | *[Dán toàn bộ mã khóa bí mật ở Bước 1 vào]* | Chìa khóa SSH riêng để GitHub kết nối |
| **`MONGODB_URI`** | `mongodb+srv://phantruong0725_db_user:VUD6BomVrUJN1Pzc@cluster0.xvq3axi.mongodb.net/todo-db?retryWrites=true&w=majority&appName=Cluster0` | Chuỗi kết nối MongoDB Atlas |

---

## 🚀 3. Cách Kích Hoạt CI/CD Tự Động Bằng Git Tag

Workflow đã được đẩy lên GitHub tại [.github/workflows/deploy.yml](file:///d:/Code/ToDo-List/backend/.github/workflows/deploy.yml).

Từ bây giờ, mỗi khi bạn sửa code Backend xong và muốn đưa lên máy chủ AWS, bạn chỉ cần gõ đúng **2 lệnh**:

```bash
cd backend

# 1. Tạo tag phiên bản mới
git tag v1.0.0

# 2. Đẩy tag lên GitHub
git push origin v1.0.0
```

Ngay lập tức:
* Bạn mở tab Actions trên GitHub: **[https://github.com/PhanTruong257/todo-be/actions](https://github.com/PhanTruong257/todo-be/actions)**
* GitHub Actions sẽ tự động SSH vào máy chủ AWS EC2, build lại Docker container và restart ứng dụng trong vòng **30 giây**!

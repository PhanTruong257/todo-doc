# 📋 Trạng Thái Tính Năng & Tiến Độ Dự Án (FEATURES.md)

Tài liệu này ghi lại toàn bộ tính năng **đã hoàn thiện**, **đang phát triển** và **dự kiến trong tương lai**.  
> 🤖 **Lưu ý cho AI Agent (Antigravity)**:  
> - Luôn đọc file này trước khi phát triển tính năng mới để tránh viết trùng hoặc phá vỡ cấu trúc hiện tại.  
> - Sau khi thêm/sửa bất kỳ tính năng nào, **bắt buộc cập nhật trạng thái** và ghi vào phần Nhật Ký (Changelog) ở cuối file.

---

## 🚀 1. Tổng Quan Tiến Độ Dự Án

| Hạng mục | Trạng thái | Ghi chú |
| :--- | :---: | :--- |
| **Kiến trúc Multi-Root Workspace (3 Git Repos)** | ✅ Hoàn thành | `todo-doc` (Root/Docs/Agents), `todo-fe` (Frontend), `todo-be` (Backend) |
| **Giai đoạn 1: Bảng Chữ Cái Hangeul (`HangeulView`)** | ✅ Hoàn thành | Lộ trình 5 ngày, IME Automata, Viết tên Hangeul, Minimal Pairs, 7 Patchim |
| **Giai đoạn 2: Từ Vựng & Lộ Trình 30 Ngày (`KoreanVocabView`)** | ✅ Hoàn thành | 240 từ lộ trình TOPIK 1, Cây phả hệ Sino-Korean, Flashcard SRS SM-2 |
| **Giai đoạn 3: Ngữ Pháp Thực Chiến (`KoreanGrammarView`)** | ✅ Hoàn thành | 16 chủ điểm ngữ pháp + Quiz, Lắp ráp câu SOV, Bảng chia động từ |
| **Giai đoạn 4: Hội Thoại & Nhập Vai (`KoreanDialogueView`)** | ✅ Hoàn thành | 60+ mẫu câu, Nhập vai 2 chiều Roleplay, Nhận diện giọng nói Web Speech, Dịch GTX |
| **Cẩm Nang Bí Quyết & Động Lực (`StudyAdviceModal`)** | ✅ Hoàn thành | 16 lời khuyên sư phạm thực chiến, châm ngôn tiếng Hàn TTS, Spotlight, lọc danh mục |
| **Gamification & Thói Quen Học Tập** | ✅ Hoàn thành | Chuỗi ngày Streak 🔥, Nhiệm vụ ngày Daily Quests, 14-Day Heatmap |
| **Kho Từ Vựng Tiếng Anh Oxford 5000 (`FlashcardView`)** | ✅ Hoàn thành | 4,954 từ vựng 13 chủ đề ngữ nghĩa, CEFR A1-C1, Flashcard 3D SRS SM-2 |
| **Giao Diện Di Động (100% Mobile Responsive)** | ✅ Hoàn thành | Mobile Bottom Nav, Header siêu mỏng 36px, Footer Glassmorphism, Zero Horizontal Scroll |
| **Backend Core (NestJS 12 + MongoDB Atlas)** | ✅ Hoàn thành | Modules Flashcards & Todos, Mongoose Schema, DTO Validation, Swagger Docs |
| **Hạ Tầng AWS & CI/CD Tự Động** | ✅ Hoàn thành | S3 + CloudFront CDN SSL (FE), EC2 Ubuntu Docker (BE), GitHub Actions deploy qua Tag |

---

## 🛠️ 2. Chi Tiết Các Tính Năng Đã Triển Khai (Done)

### 🔹 Phân Hệ 1: Giai Đoạn 1 - Bảng Chữ Cái & Phát Âm Hangeul (`HangeulView`)
- [x] **Lộ Trình 5 Ngày Nền Tảng**:
  - **Ngày 1**: 9 ký tự nền tảng (6 nguyên âm `ㅏ, ㅓ, ㅗ, ㅜ, ㅡ, ㅣ` + 3 phụ âm `ㅇ, ㄴ, ㅁ`) & 7 từ vựng đầu tiên (`아이`, `오이`, `우유`...).
  - **Ngày 2**: 7 phụ âm phát âm tự nhiên (`ㄱ, ㄷ, ㅂ, ㅅ, ㅈ, ㄹ, ㅎ`) & từ vựng đồ vật quen thuộc (`나무`, `모자`, `바지`...).
  - **Ngày 3**: Nguyên lý thêm nét Sejong (`ㄱ➔ㅋ`, `ㄷ➔ㅌ`, `ㅂ➔ㅍ`) & Từ mượn quốc tế Konglish (`커피`, `피자`, `카메라`...).
  - **Ngày 4**: Phụ âm căng (`ㄲ, ㄸ, ㅃ, ㅆ, ㅉ`) & Từ vựng gia đình thân thuộc (`아빠`, `오빠`...).
  - **Ngày 5**: Đánh vần Patchim (phụ âm cuối) đối chiếu vần tiếng Việt (`-c, -t, -p, -m, -n, -ng`) ➔ Đọc thông câu chào (`안녕하세요!`).
- [x] **Quy Trình Học 3 Bước Mỗi Ngày**:
  - *Bước 1 - Âm trước hình sau*: Hình ảnh tượng hình, mẹo nhớ trực quan và audio giọng bản xứ `ko-KR`.
  - *Bước 2 - Từ vựng thực tế*: Đọc các từ vựng thật ngoài đời thực, kèm phân tích cấu tạo ghép âm tiết.
  - *Bước 3 - Thử thách phản xạ*: Trắc nghiệm phản xạ nhận diện âm thanh và mặt chữ, chấm điểm và giải thích tức thì.
- [x] **Bộ Giả Lập Gõ IME Automata 2-Set (HangulImeAutomata)**:
  - Ghép âm tiết động Unicode theo công thức `(Choseong * 21 + Jungseong) * 28 + Jongseong + 0xAC00`.
  - Tự động đẩy phụ âm cuối sang âm tiết tiếp theo khi gặp nguyên âm (ví dụ: `먹` + `ㅓ` ➔ `머거`).
  - Hỗ trợ ghép nguyên âm phức (`ㅗ + ㅏ = ㅘ`) và patchim kép (`ㄱ + ㅅ = ㄳ`).
  - Hỗ trợ bàn phím ảo chạm màn hình và phím vật lý máy tính (ánh xạ `event.code` triệt tiêu xung đột gõ tiếng Việt Telex/VNI).
- [x] **Tự Viết Tên Bạn Bằng Chữ Hangeul ✍️**:
  - Danh bạ 50+ tên tiếng Việt thông dụng (`Trường`, `Nguyệt`, `Anh`, `Bình`, `Dũng`, `Hương`, `Linh`, `Nam`, `Phương`...).
  - Phiên âm chuẩn Hán-Hàn, phân tích ý nghĩa, phát âm giọng đọc bản xứ và tạo Name Card chia sẻ.
- [x] **Cặp Âm Tối Thiểu (Minimal Pairs)**:
  - Luyện phân biệt bộ ba phụ âm: Thường (`ㅂ/ㅈ/ㄱ`) vs Bật hơi (`ㅍ/ㅊ/ㅋ`) vs Căng (`ㅃ/ㅉ/ㄲ`) qua các cặp từ đối chiếu (`불/풀/뿔`, `자다/차다/짜다`, `달/탈/딸`).
  - Phân biệt nguyên âm dễ nhầm: `ㅓ/ㅗ` (`어머니/오빠`), `ㅡ/ㅜ` (`음식/우유`).
- [x] **7 Nhóm Âm Đại Diện Patchim & Quy Tắc Biến Âm**:
  - Bài học 7 nhóm phụ âm cuối đại diện (`ㄱ, ㄴ, ㄷ, ㄹ, ㅁ, ㅂ, ㅇ`).
  - 4 quy tắc biến âm cốt lõi: Nối âm (Liaison), Mũi hóa (Nasalization), Căng âm (Tensification), Bật hơi (Aspiration) kèm đối chiếu dạng viết vs dạng đọc thực tế (`[함니다]`, `[머거요]`).
- [x] **Công Tắc Ẩn/Hiện Phiên Âm Latin (Romaja Toggle 👁️)**: Lưu trạng thái `localStorage` trên toàn hệ thống giúp người học cai phụ thuộc Latin, ép phản xạ trực tiếp trên chữ Hangeul.

---

### 🔹 Phân Hệ 2: Giai Đoạn 2 - Kho Từ Vựng & Lộ Trình 30 Ngày TOPIK 1 (`KoreanVocabView`)
- [x] **Giáo Trình Lộ Trình 30 Ngày TOPIK 1 (240 Từ Vựng)**:
  - Phân bổ qua 5 tuần học: Tuần 1 (Sinh tồn & Cá nhân), Tuần 2 (Hành động đời sống), Tuần 3 (Cảm xúc & Thời tiết), Tuần 4 (Số đếm & Mua sắm), Tuần 5 (Liên từ & Về đích).
  - Cấu trúc bài học ngày: Mục tiêu (Focus) + Mẹo ngữ pháp (Grammar Tip) + Lời khuyên ghi nhớ thực chiến (Quick Advice).
  - Thẻ từ vựng đa tầng: Hangeul lớn, phát âm `ko-KR`, phiên âm Latin, biến âm thực tế, liên kết âm Hán - Việt tương đương và câu ví dụ song ngữ.
  - Thẻ xem trước ngày mai (Tomorrow Preview) tạo động lực học tập liên tục.
- [x] **Cây Phả Hệ Âm Hán-Hàn ("Học 1 Biết 10" - `SinoKoreanView`)**:
  - Tra cứu 12+ gốc Hán-Hàn cốt lõi (`학 - HỌC`, `생 - SINH`, `인 - NHÂN`, `국 - QUỐC`, `원 - VIÊN`, `식 - THỰC`, `화 - HÓA`, `전 - ĐIỆN`, `동 - ĐỘNG`, `실 - THẤT`, `문 - VĂN`, `기 - KHÍ`).
  - Bung ra mạng lưới từ ghép phái sinh trực quan kèm giải nghĩa Hán-Việt tương đồng.
  - Mini Game ghép 2 gốc chữ sinh từ vựng có chấm điểm phản xạ.
- [x] **Kho Từ Vựng TOPIK 1 Chuyên Sâu (124+ Từ Vựng)**:
  - Phân loại 8 chủ đề thiết yếu (Chào hỏi, Đồ vật, Gia đình, Đồ ăn, Địa điểm, Thời gian, Hoạt động, Cảm xúc).
  - Đầy đủ thông tin: Từ loại POS, dạng từ điển & dạng chia đuôi câu (`-아요/어요`), phân loại từ gốc Hán / thuần Hàn / bạn giả (False Friends: `인사`, `방학`, `공부`, `기분`, `약속`).
- [x] **Hệ Thống Ôn Tập Flashcard SRS SuperMemo-2 (SM-2)**:
  - 4 nút đánh giá khách quan: Quên (`Again`), Khó (`Hard`), Tốt (`Good`), Dễ (`Easy`).
  - Thuật toán SuperMemo-2 tính hệ số ghi nhớ Ease Factor (`EF` điều chỉnh 1.3 - 3.0) và khoảng cách ngày ôn lũy tiến.
  - 3 chế độ học Active Recall: `Hàn ➔ Việt` (Nhận diện), `Việt ➔ Hàn` (Phản xạ/Gợi nhớ), và `Nghe ➔ Chọn` (Luyện nghe).
  - Trắc nghiệm 4 đáp án khách quan trên mặt trước thẻ trước khi lật, chống tự chấm "Good" ảo.
  - Chế độ gõ từ vựng (Type-In Recall) kiểm tra khả năng nhớ chính xác mặt chữ.

---

### 🔹 Phân Hệ 3: Giai Đoạn 3 - Sổ Tay Ngữ Pháp Thực Chiến (`KoreanGrammarView`)
- [x] **16 Chủ Điểm Ngữ Pháp Cốt Lõi**:
  - Động từ `이다` (là) / `아니다` (không phải là).
  - Tồn tại `있다` (có/ở) / `없다` (không có/không ở).
  - Đại từ chỉ định `이/그/저` (này / đó / kia).
  - Trợ từ chủ ngữ `은/는` (nhấn mạnh đề tài/đối chiếu) vs `이/가` (nhấn mạnh chủ ngữ/thông tin mới).
  - Trợ từ tân ngữ `을/를`.
  - Trợ từ nơi chốn/thời gian `에` (hướng đến/thời điểm) vs `에서` (hành động diễn ra tại nơi chốn).
  - Đuôi câu thân mật lịch sự `-아요/어요`.
  - Đuôi câu trang trọng `-습니다/습니까`.
  - Đuôi câu mong muốn `-고 싶다`.
  - Đuôi câu mệnh lệnh & yêu cầu `-(으)세요` / `-아/어 주세요`.
  - Phủ định `안` & `-지 않다`.
  - Quá khứ `-았/었어요`.
  - Tương lai `-(으)ㄹ 거예요`.
  - Số thuần Hàn & Số Hán-Hàn kèm danh từ chỉ đơn vị đếm (`명`, `개`, `병`, `권`...).
  - Liên từ nối câu (`và`, `nhưng`, `vì... nên`).
- [x] **Bài Tập Trắc Nghiệm & Điền Khuyết Tự Động Chấm**: Mỗi bài ngữ pháp tích hợp bài tập tương tác, hiển thị kết quả và giải thích quy tắc sư phạm tức thì.
- [x] **Trình Lắp Ráp Câu SOV & Luyện Trợ Từ (`SentenceBuilderView`)**:
  - 10+ câu tình huống đời sống luyện trật tự câu Chủ - Nơi chốn - Tân ngữ - Động từ.
  - Trắc nghiệm chọn đúng trợ từ gắn vào từng thành phần câu kèm giải thích chi tiết.
- [x] **Bảng Tra Cứu & Mini Drill Chia Động Từ (`VerbConjugatorView`)**:
  - Tra cứu 18+ động từ thông dụng, hiển thị bảng 7 thể chia (thân mật, trang trọng, quá khứ, tương lai, phủ định, mong muốn, kính trọng).
  - Phân tích và chỉ rõ các dạng bất quy tắc phổ biến (`ㅂ, ㄷ, ㅡ, 르`).
  - Mini Drill thử thách 60 giây luyện phản xạ chia nhanh động từ.

---

### 🔹 Phân Hệ 4: Giai Đoạn 4 - Hội Thoại Giao Tiếp & Nhập Vai (`KoreanDialogueView`)
- [x] **60+ Mẫu Câu Giao Tiếp Sống Còn**: Phân bổ theo 5 tình huống thực tế (Chào hỏi, Mua sắm & Thanh toán, Ăn uống & Nhà hàng, Hỏi đường & Di chuyển, Khẩn cấp).
- [x] **Hội Thoại Nhập Vai 2 Chiều (`RoleplayView`)**:
  - 3 kịch bản thực chiến: Gọi cà phê tại Mega Coffee, Mua đồ tại cửa hàng tiện lợi CU, Đi Taxi tại Seoul.
  - Giọng đọc thoại NPC bản xứ, phân nhánh câu trả lời thực tế theo ngữ cảnh.
- [x] **Nhận Diện Giọng Nói Micro (Web Speech Recognition)**: Thu âm phát âm tiếng Hàn `ko-KR` của người học và chấm điểm độ chính xác phát âm theo thời gian thực.
- [x] **Công Cụ Dịch 2 Chiều Hàn ⇄ Việt (Google GTX API)**:
  - Dịch câu tự do 2 chiều Hàn ⇄ Việt, phát âm audio giọng đọc bản xứ.
  - Nút 1-chạm lưu câu dịch thành thẻ Flashcard cá nhân.
  - Modal cảnh báo bản dịch máy (Safe Machine Translation) cho phép duyệt và chỉnh sửa trước khi lưu.

---

### 🔹 Phân Hệ 5: Cẩm Nang Bí Quyết & Động Lực Học Tập (`StudyAdviceModal`)
- [x] **16 Lời Khuyên Sư Phạm Thực Chiến & Châm Ngôn Truyền Lửa**:
  - *Tâm Lý & Động Lực*: Quy tắc 15 phút vàng, giải mã đường cong quên Ebbinghaus, vượt qua nỗi sợ nói sai, nuôi dưỡng động lực ghi nhận từng tiến bộ nhỏ.
  - *Chữ Cái & Phát Âm*: Lợi thế phát âm của người Việt, cai Romaja, mẹo hình học Âm-Dương, mẹo nhỏ với tờ giấy mỏng kiểm tra hơi bật.
  - *Gốc Hán & Từ Vựng*: Mỏ vàng Hán-Hàn "học 1 biết 10", Active Recall 3 giây, học cụm từ Collocation, quy tắc 24h vàng sau buổi học.
  - *Ngữ Pháp & Phản Xạ*: Tư duy đảo ngược SOV, bí kíp phân biệt `-은/는` vs `-이/가`, kỹ thuật Shadowing đọc đuổi cùng người bản xứ, độc thoại tự nói một mình.
- [x] **Giao Diện Modal Kính Mờ Glassmorphism Cao Cấp**:
  - Thẻ Spotlight lời khuyên ngẫu nhiên mỗi ngày 🎲.
  - Phát âm châm ngôn tiếng Hàn bằng TTS `ko-KR`.
  - Bộ lọc danh mục chip vuốt ngang và ô tìm kiếm từ khóa thời gian thực.
  - Đánh dấu thói quen đã áp dụng ✅ kèm thanh tiến độ làm chủ (`localStorage`).
  - Nút 1-chạm sao chép chia sẻ lời khuyên.

---

### 🔹 Phân Hệ 6: Gamification, Thói Quen & Flashcard SRS Tiếng Anh
- [x] **Chuỗi Ngày Học Liên Tục (Streak 🔥)**: Tính toán tự động theo ngày lịch thực tế và hiển thị huy hiệu ngọn lửa nhấp nháy trên Header và Modal.
- [x] **Nhiệm Vụ Hàng Ngày (`DailyQuestsModal`)**: 3 nhiệm vụ ngày (Học 5 thẻ mới, Ôn tập 5 thẻ, Học 1 bài ngữ pháp) có thanh tiến độ và ma trận chăm chỉ 14 ngày (14-Day Heatmap).
- [x] **Kho Từ Vựng Flashcards Tiếng Anh Oxford 5000 (`FlashcardView`)**:
  - 4,954 từ vựng Oxford 3000 & 5000 phân chia theo 13 chủ đề ngữ nghĩa và cấp độ CEFR A1-C1.
  - Thẻ 3D Flashcard hiệu ứng lật không gian chiều sâu, phát âm audio MP3 / Web Speech API.
  - Tự động tra nghĩa từ vựng qua Free Dictionary API và lưu vào MongoDB Atlas.

---

### 🔹 Phân Hệ 7: UI/UX & Mobile-First Optimization
- [x] **Menu Dropdown Ngoài Header**: Tinh giản 4 giai đoạn học vào 1 nút dropdown kính mờ trên Header, giải phóng hơn 140px diện tích hiển thị dọc.
- [x] **Mobile Header Siêu Mỏng (~36px)**: Tối ưu cho màn hình điện thoại dưới 900px/1024px, logo nhỏ gọn, thanh chuyển tab icon tinh tế.
- [x] **Mobile Bottom Navigation Bar (`MobileBottomNav`)**: Thanh điều hướng chuẩn ứng dụng di động iOS/Android, hỗ trợ safe area, chuyển nhanh 4 giai đoạn bằng ngón cái.
- [x] **Footer Glassmorphism Cao Cấp (`Footer.tsx`)**: Logo Hangeul `한`, lối tắt 4 giai đoạn, chip tính năng trọng tâm, hộp châm ngôn tiếng Hàn, nút mở cẩm nang lời khuyên, nút Cuộn lên đầu trang mượt mà.
- [x] **Chuẩn Mobile 100% (Zero Horizontal Scroll)**: Đảm bảo tương thích hoàn mỹ trên mọi kích cỡ di động (< 1024px, < 768px, < 480px, < 360px). Kích thước chạm cảm ứng $\ge 36\text{px} - 44\text{px}$.

---

### 🔹 Phân Hệ 8: Backend & Cơ Sở Dữ Liệu (NestJS + MongoDB Atlas)
- [x] **Flashcards Module**:
  - Cung cấp API CRUD từ vựng flashcard, API tra cứu từ điển tự động (`/api/flashcards/lookup`).
  - Endpoint cập nhật SuperMemo-2 (`POST /api/flashcards/:id/review`).
- [x] **Todos Module**:
  - RESTful API CRUD công việc, lọc theo trạng thái/độ ưu tiên/tìm kiếm, API thống kê (`/api/todos/stats`).
- [x] **Mongoose Schema & Validation**: DTO đầy đủ validation decorators (`class-validator`), Swagger OpenAPI docs tại `/api/docs`.
- [x] **MongoDB Atlas**: Kết nối cụm AWS Singapore (`ap-southeast-1`), gói Free M0 512MB, tự động map `_id` sang `id`.

---

### 🔹 Phân Hệ 9: Hạ Tầng Triển Khai AWS & Multi-Root Git
- [x] **Frontend S3 + CloudFront**: Live tại `https://d3joa566sl3ve2.cloudfront.net` (SSL CDN tốc độ cao, hỗ trợ Single Page Application routing).
- [x] **Backend EC2 Docker**: Chạy live trên Ubuntu 24.04 Docker (`http://13.228.71.202:4000`).
- [x] **CloudFront Reverse Proxy (Single Domain)**: Định tuyến `/*` sang S3 và `/api/*` sang EC2, giải quyết triệt để lỗi Mixed Content và CORS.
- [x] **CI/CD Tự Động (GitHub Actions)**: Kích hoạt tự động khi push tag `v*` cho cả Frontend và Backend.
- [x] **Multi-Root Git Workspace (3 Repositories)**:
  - `todo-doc` (`D:\Code\ToDo-List`): Quản lý tài liệu và cấu hình AI Agent.
  - `todo-fe` (`source/frontend`): Mã nguồn Frontend React 19 TS.
  - `todo-be` (`source/backend`): Mã nguồn Backend NestJS 12.
  - Cấu hình qua file `taskflow.code-workspace`.

---

## 🔮 3. Kế Hoạch Tính Năng Tiếp Theo (Backlog / Upcoming)

### Giai đoạn 2: Tính Năng Nâng Cao
- [ ] **Kéo thả sắp xếp (Drag & Drop)**: Sử dụng `@hello-pangea/dnd` hoặc HTML5 Drag and Drop API để sắp xếp thứ tự ưu tiên.
- [ ] **Thông báo Toast**: Hiển thị popup thông báo góc màn hình khi thêm/sửa/xóa thành công.
- [ ] **Phân trang hoặc Virtual Scrolling**: Khi danh sách có hơn 100 công việc.

### Giai đoạn 3: Xác Thực Người Dùng (Auth & Multi-user)
- [ ] **Auth Module (Backend)**: Đăng ký, đăng nhập với JWT và mã hóa mật khẩu bằng `bcrypt`.
- [ ] **Gắn User ID vào Todo**: Mỗi người dùng chỉ thấy và quản lý Todo của riêng mình.
- [ ] **Giao diện Login/Register (Frontend)**: Màn hình đăng nhập và lưu JWT vào cookies/localStorage.

### Giai đoạn 4: Triển Khai Cloud & CI/CD
- [x] **AWS EC2 + Docker (Backend)**: Chạy Backend NestJS container trên Ubuntu EC2 (IP: `13.228.71.202:4000` & Port `80`).
- [x] **Backend CI/CD**: Workflow GitHub Actions tự động SSH vào EC2 rebuild Docker khi push tag `v*`.
- [x] **Frontend CI/CD**: Workflow GitHub Actions tự động build Vite và đồng bộ AWS S3 + CloudFront khi push tag `v*`.
- [x] **AWS S3 + CloudFront (Frontend)**: Đã triển khai live tại `https://d3joa566sl3ve2.cloudfront.net` (Distribution: `E3KXJQMP6ZWPPM`, Bucket: `taskflow-fe-phan`).
- [x] **Kiến trúc Single Domain Reverse Proxy (Enterprise)**: CloudFront định tuyến `/*` sang S3 và `/api/*` sang EC2, giải quyết 100% lỗi Mixed Content & CORS, bảo vệ an toàn IP máy chủ gốc.

---

## 📝 4. Nhật Ký Thay Đổi (Changelog)

| Ngày | Người thực hiện | Nội dung thay đổi |
| :---: | :---: | :--- |
| **2026-10-10** | Antigravity AI + User | Khởi tạo Fullstack FE (React TS) + BE (NestJS) + MongoDB Atlas AWS (512MB M0). |
| **2026-10-10** | Antigravity AI + User | Tách 2 Git repo riêng `todo-fe` và `todo-be`, đẩy lên GitHub. |
| **2026-10-10** | Antigravity AI + User | Tái cấu trúc Frontend theo chuẩn **Co-location Component**: Gom `Component.tsx`, `useComponent.ts` và `Component.css` chung 1 folder. |
| **2026-10-10** | Antigravity AI + User | Tạo tài liệu theo dõi tiến độ `FEATURES.md` và gắn vào `AGENTS.md`. |
| **2026-10-11** | Antigravity AI + User | Triển khai thành công Backend NestJS Docker Container lên **AWS EC2** (IP: `13.228.71.202:4000`), kết nối live với **MongoDB Atlas**. |
| **2026-10-11** | Antigravity AI + User | Thiết lập hoàn chỉnh bộ đôi **GitHub Actions CI/CD** cho cả **Frontend** (S3 + CloudFront) và **Backend** (EC2 SSH Docker) kích hoạt tự động qua Git Tag (`v*`). |
| **2026-10-11** | Antigravity AI + User | Triển khai kiến trúc **CloudFront Reverse Proxy** (Single Domain): Định tuyến `/api/*` về EC2, phát hành Frontend `v1.0.1` với kết nối live API qua HTTPS hoàn hảo. |
| **2026-10-11** | Antigravity AI + User | Triển khai tính năng **Báo Cáo Học Tiếng Anh Hàng Ngày**: Báo cáo riêng biệt cho Trường & Nguyệt + thêm người khác linh hoạt, chọn giờ bắt đầu/kết thúc (tự tính thời lượng), ghi chép nội dung bài học. Deploy tự động qua CI/CD Backend `v1.0.1` & Frontend `v1.0.2`. |
| **2026-10-11** | Antigravity AI + User | Tái thiết kế toàn diện UI (Local First): Tách **Sidebar học sinh bên trái** (chuyển đổi Trường / Nguyệt / Tất cả), thanh điều hướng Header (Báo Cáo / Thống Kê), thanh **Lịch Tuần Trực Quan (CalendarStrip)** lọc báo cáo theo ngày click, và bảng **Thống Kê Chuyên Sâu (StatsView)**. |
| **2026-10-11** | Antigravity AI + User | Chuyển đổi hoàn toàn sang mô hình **Báo Cáo / Nhật Ký Học Tập Thuần Túy** (Loại bỏ các khái niệm Todo List): Bỏ nút tròn checkbox hoàn thành trên thẻ, bỏ các tab lọc todo `Đang học` / `Đã xong`, bỏ chữ `đã xong` ở sidebar; tích hợp ô chọn ngày popup trực tiếp trên Header và nút thêm `+` tối giản. |
| **2026-10-11** | Antigravity AI + User | Tối ưu hóa Date Picker: Thay thế `<input type="date">` mặc định của trình duyệt bằng **Custom Glassmorphic DatePicker Component** (`DatePicker.tsx`, `DatePicker.css`, `useDatePicker.ts`). Bấm bất kỳ vị trí nào trên pill đều mở popup, hiển thị lịch tháng trực quan, có chấm xanh báo hiệu ngày có buổi học, hỗ trợ phím tắt `Hôm nay`, `Hôm qua`, `Tất cả ngày`. |
| **2026-10-11** | Antigravity AI + User | Tối ưu hóa Form Thêm Báo Cáo: Tự động gán người học theo học viên đang chọn (click vào Trường thì form tự động gán cho Trường, click vào Nguyệt thì tự động gán cho Nguyệt); loại bỏ hàng nút chọn thủ công học viên trong form để tiết kiệm thao tác. |
| **2026-10-11** | Antigravity AI + User | Tối ưu hóa Trang Thống Kê: Loại bỏ hoàn toàn các khái niệm Todo như "Tỉ lệ hoàn thành 0%", "Đã xong 0 buổi", thanh progress bar; thay thế bằng các chỉ số báo cáo chuẩn: **Tổng thời gian học**, **Tổng số buổi học**, **Thời lượng trung bình mỗi buổi**, và **Danh sách các chủ đề đã học**. |
| **2026-10-11** | Antigravity AI + User | Tối ưu hóa Z-Index & Độ Ưu Tiên Hiển Thị Lịch (DatePicker Popup): Cấu hình `position: relative` và `z-index: 500` cho `Header`, `z-index: 99999` cho DatePicker popup cùng gradient nền tối; đảm bảo lịch khi mở ra luôn nổi lên trên cùng, không bị che khuất hay cắt đè bởi thanh tìm kiếm và danh sách nội dung bên dưới. |
| **2026-10-11** | Antigravity AI + User | Bổ sung Rule & Tối ưu hóa toàn diện giao diện Mobile (Mobile Responsive): Thêm quy tắc cốt lõi số 7 vào `AGENTS.md` & `project-rules.md`. Tinh chỉnh toàn bộ CSS (`App.css`, `index.css`, `StudentSidebar.css`, `Header.css`, `DatePicker.css`, `FilterBar.css`, `TodoInput.css`, `TodoItem.css`, `StatsView.css`) hỗ trợ mượt mà từ màn hình 320px đến 768px: chống tràn ngang (`overflow-x: hidden`), sidebar biến thành thanh vuốt ngang pill học sinh tiện lợi, popup lịch căn giữa linh hoạt theo màn hình, nút bấm và form tối ưu kích cỡ chạm (touch-friendly). |
| **2026-10-11** | Antigravity AI + User | Tái thiết kế toàn diện Header Mobile (Unified 3-Row Mobile Header): Thay thế hoàn toàn 3 khối card cồng kềnh xếp chồng (Sidebar -> Header -> FilterBar) bằng component duy nhất `MobileHeader.tsx` (tiết kiệm hơn 300px chiều cao). Hàng 1 (Logo + Tab Báo Cáo / Thống Kê), Hàng 2 (Vuốt ngang chọn học viên Tất cả / Trường / Nguyệt / +), Hàng 3 (Lịch ngày + Tìm kiếm + Nút +). Thẻ báo cáo học tập lập tức hiển thị nổi bật trên màn hình đầu tiên. |
| **2026-10-11** | Antigravity AI + User | Tối giản Trang Thống Kê (StatsView): Loại bỏ hoàn toàn khối 3 thẻ tổng hợp chung (Tổng thời gian học, Tổng số buổi, Trung bình mỗi buổi) trên cả Laptop và Mobile; trang thống kê tập trung trực tiếp và rõ ràng vào từng học viên cụ thể (Trường / Nguyệt). |
| **2026-10-11** | Antigravity AI + User | Cố định 2 thanh điều hướng trên Laptop & Mobile (Sticky Bars): Sửa triệt để lỗi vỡ layout trên desktop; cấu hình cố định tuyệt đối cho **Thanh 1 (Sidebar học sinh bên trái)** (`sticky top: 20px`, chiều cao tự co giãn `calc(100vh - 40px)`) và **Thanh 2 (Cụm Header + FilterBar bên trên)** (`sticky top: 20px`, `z-index: 500`). Khi cuộn danh sách hàng chục bài học, cả 2 thanh đều ghim chắc chắn, không bị trôi hay vỡ giao diện. Đồng bộ breakpoint 900px mượt mà giữa Desktop và Mobile. |
| **2026-10-11** | Antigravity AI + User | Tối ưu hóa Toàn Màn Hình (Full-Height Layout): Xử lý hiện tượng giao diện bị co rúm vào nửa trên màn hình để lộ khoảng đen bên dưới khi có ít bài học. Cấu hình `min-height: 100vh` cho layout chính, Sidebar bên trái mở rộng trọn vẹn toàn bộ chiều cao cửa sổ (`height: calc(100vh - 56px)` kèm chân trang đồng bộ), đẩy Footer xuống sát đáy màn hình (`margin-top: auto`), tăng `max-width: 1440px` giúp giao diện mở rộng thoáng đãng, sang trọng trên mọi màn hình máy tính. |
| **2026-10-11** | Antigravity AI + User | Kéo dài Sidebar Tràn Đáy 100% (Full-Height Sidebar): Xóa thuộc tính `height: fit-content` giới hạn trước đó trong `StudentSidebar.css`; đặt `height: 100%` cùng `justify-content: space-between` và thanh danh sách học viên có thể cuộn riêng (`overflow-y: auto`), giúp thanh Sidebar bên trái kéo dài hết toàn bộ chiều cao cửa sổ từ trên xuống dưới một cách chuyên nghiệp. |
| **2026-10-11** | Antigravity AI + User | Chuyển đổi Báo Cáo Học Viên trong Thống Kê sang Dạng Danh Sách Nằm Ngang (Horizontal Row List): Thay thế bố cục grid card 2 cột bằng danh sách các dòng nằm ngang thanh thoát (`.student-analytics-row`). Mỗi học viên là 1 dòng duy nhất gồm 4 cột (Profile học viên -> 3 chỉ số thời lượng/buổi học -> Chủ đề đã học -> Nút Xem báo cáo), có hover hiệu ứng viền màu phân biệt (Xanh cho Trường, Hồng cho Nguyệt) và tự động co giãn thông minh trên màn hình nhỏ. |
| **2026-10-11** | Antigravity AI + User | Tự động Gán Thời Gian Hiện Tại & Hỗ trợ Nhập Giờ 24h Chuẩn (TimePicker24h): Khắc phục hạn chế của `<input type="time">` trình duyệt chỉ cho nhập 12h (AM/PM) và chặn gõ giờ 20h. Xây dựng component `TimePicker24h`: tự động gán giờ bắt đầu là **thời điểm hiện tại** (`getCurrentTime24h()`) và giờ kết thúc sau 60 phút khi mở form; cho phép gõ trực tiếp giờ 24h (ví dụ `20`, `20:00`, `20:30`...) tự động chuẩn hóa; tích hợp popover kính mờ chọn nhanh mốc giờ 24h, nút `Bây giờ` và các nút tăng giảm nhanh `+15m`, `+30m`, `+1h`. Áp dụng đồng bộ cho cả form thêm mới và chế độ sửa buổi học. |
| **2026-10-11** | Antigravity AI + User | Triển khai Bộ 5 Tiện Ích Đột Phá Cho Trải Nghiệm Học Tập: (1) **Chuỗi ngày học liên tục (Streak 🔥)** tính toán theo ngày lịch thực tế và hiển thị huy hiệu ngọn lửa nhấp nháy trên Sidebar, Mobile Header và Thống kê; (2) **Tag kỹ năng chọn nhanh (Quick Skill Badges 🏷️)** với 8 pill 1 chạm điền tức thì Speaking, Writing, Listening, Reading, Từ vựng, Ngữ pháp; (3) **Đồng hồ bấm giờ học trực tiếp (Study Timer ⏱️)** lưu trạng thái vào localStorage (không bị mất khi reload), tự động tính đúng từng phút và chuyển thẳng vào form báo cáo; (4) **Biểu đồ cột hoạt động theo tuần (Weekly Activity Chart 📈)** hiển thị trực quan 7 ngày từ Thứ 2 đến Chủ Nhật kèm thời lượng và chỉ báo Hôm nay; (5) **Cài đặt như App điện thoại (PWA 📲)** với Web App Manifest, icon vector chuẩn và Service Worker sẵn sàng cài lên màn hình chính. |
| **2026-10-11** | Antigravity AI + User | Tối ưu Trải Nghiệm Thêm Buổi Học (Mục 2 & Mục 3): (1) **Chọn Thời Lượng 1-Chạm (Quick Duration Presets ⚡)**: Bổ sung hàng nút chọn nhanh `15 phút`, `30 phút`, `45 phút`, `1 tiếng`, `1.5 tiếng` tự động tính chính xác giờ kết thúc và cập nhật thời lượng tức thì; (2) **Bảng Thêm Trượt Đáy Kiểu iOS (iOS Bottom Sheet 📲)**: Chuyển toàn bộ form thêm buổi học thành overlay trượt từ đáy màn hình nhô lên kèm vạch kéo iOS, hỗ trợ vuốt ngón tay xuống để đóng, bấm ngoài vùng mờ hoặc phím Escape để thoát, khóa cuộn trang nền khi mở form, hiển thị dạng modal kính mờ sang trọng trên desktop. |
| **2026-10-11** | Antigravity AI + User | Tích hợp Đồng Hồ Đếm Ngược (Countdown Timer / Pomodoro) & Chuông Báo Ting: (1) Hỗ trợ chuyển đổi mượt mà 2 chế độ: **Đếm ngược (Countdown)** và **Bấm giờ (Stopwatch)**; (2) Mốc đếm ngược 1-chạm: `15p`, `25p (Pomodoro)`, `30p`, `45p`, `60p`; (3) Hiển thị thanh tiến độ co dần trực quan; (4) Tự động phát âm thanh chuông "Ting! 🔔" trong trẻo qua Web Audio API (không trễ, không phụ thuộc mp3 ngoài) kèm rung điện thoại khi hết giờ; (5) Banner chúc mừng hoàn thành và nút bấm 1 chạm đưa kết quả vào form báo cáo học tập. |
| **2026-10-11** | Antigravity AI + User | Tối Giản Form Báo Cáo: Bỏ ràng buộc bắt buộc (`required`) cho ô nội dung chi tiết bài học; khi chỉ cần nhập hoặc chọn chủ đề buổi học là đã có thể bấm lưu báo cáo ngay lập tức, tiết kiệm tối đa thời gian ghi nhật ký. |
| **2026-10-11** | Antigravity AI + User | Chuẩn Hóa Thuật Ngữ Toàn Diện: Thay thế toàn bộ từ ngữ mang tính trường lớp "Buổi học" thành **"Nhật Ký Học"** (hoặc "Nhật ký") trên toàn bộ hệ thống (Form thêm, Sidebar học viên, Thẻ báo cáo, Thống kê, Lịch tuần, Lọc tìm kiếm), phản ánh chính xác tinh thần tự học tiếng Anh mỗi ngày của người dùng. |
| **2026-10-11** | Antigravity AI + User | Đóng Gói Danh Sách Nhật Ký Trong Khung Cuộn Độc Lập (Scrollable List Container): Bọc toàn bộ danh sách thẻ nhật ký vào thẻ div `.todo-list-scroll-wrap` có chiều cao giới hạn tự co giãn thông minh (`calc(100dvh - 225px)` trên desktop và `calc(100dvh - 200px)` trên mobile); chỉ cuộn nội dung bên trong với thanh cuộn kính mờ tùy chỉnh, giữ cố định tuyệt đối Header, Thanh lọc và Sidebar trên màn hình. |
| **2026-10-11** | Antigravity AI + User | Tối Giản Kích Thước Thẻ Nhật Ký (Compact Card Design): Tái cấu trúc thẻ nhật ký thành 1 hàng ngang duy nhất siêu thanh thoát (giảm padding từ 18px xuống 10px); gom Badge học viên + Tiêu đề bài học bên trái, Thời lượng + Nút Sửa/Xóa tích hợp thẳng bên phải; chỉ mở rộng chiều cao khi có nội dung ghi chép chi tiết, giúp màn hình hiển thị được gấp đôi số lượng nhật ký cùng lúc. |
| **2026-10-11** | Antigravity AI + User | Tinh Giản Thẻ Tối Đa: Bỏ hoàn toàn badge tên học viên trên thẻ (nhận diện qua viền màu bên trái); bỏ nhãn chữ `Nội dung:`, hiển thị thẳng nội dung ghi chép dạng inline nhẹ nhàng dưới tiêu đề khi có dữ liệu. |
| **2026-10-11** | Antigravity AI + User | Tìm Kiếm Tiếng Việt Thông Minh (Accent-Insensitive Search): Chuẩn hóa thuật toán tìm kiếm (`matchesSearch`) hỗ trợ tìm kiếm không dấu (ví dụ: gõ `ho` ra `học`, gõ `truong` ra `Trường`, gõ `de mock` ra `Đề Mock`); hỗ trợ khớp đa từ linh hoạt với độ trễ 0ms thời gian thực. |
| **2026-10-11** | Antigravity AI + User | Chuẩn Hóa Ngôn Từ Cá Nhân Hóa (Nhật Ký & Thành Viên): Loại bỏ triệt để các thuật ngữ mang tính sư phạm trường lớp ("Học viên", "Người học", "Báo cáo", "Buổi học") chuyển thành **"Thành viên"**, **"Nhật Ký Học"**, **"Nhật ký"** trên toàn bộ hệ thống (`Sidebar`, `Header`, `StatsView`, `StatsCards`, `TodoInput`, `TodoItem`, `CalendarStrip`, `Footer`). |
| **2026-10-11** | Antigravity AI + User | Gom Toàn Bộ Vùng Phải Vào 1 Vùng Scroll Duy Nhất (Single Right-Side Scroll Area): Sửa triệt để lỗi vỡ UI và cuộn nhầm sidebar. Cấu hình trên Desktop: (1) Khóa scroll cửa sổ (`overflow: hidden; height: 100vh`); (2) Sidebar bên trái cố định 100% chiều cao không bao giờ bị cuộn; (3) Toàn bộ vùng bên phải (`.app-main-content`) trở thành vùng cuộn duy nhất mượt mà (`overflow-y: auto`), thanh Header dính cố định ở đỉnh vùng cuộn (`position: sticky; top: 0`), sửa lỗi cắt nửa pill chủ đề (`topics-pills`) và hỗ trợ responsive mượt mà dưới 1200px trong Thống Kê. |
| **2026-10-11** | Antigravity AI + User | Chuẩn Hóa Bố Cục Chuẩn Top-Header + Sidebar + Content (Strict Viewport Containment): Tái cấu trúc giao diện theo mô hình kiến trúc SaaS tiêu chuẩn: (1) **Header luôn ở trên top** chạy suốt chiều ngang (tích hợp Logo & Brand, Ngữ cảnh thành viên, DatePicker và Tabs); (2) Bên dưới Header là khối thân chia đôi: **Bên trái là Sidebar thành viên** (cố định, chiều cao vừa khít `height: 100%`), **Bên phải là Content** (chiều cao vừa khít `height: 100%`); (3) **Tuyệt đối không bao giờ nội dung vượt quá chiều cao màn hình** (`100vh`), mọi thao tác cuộn chỉ diễn ra nội bộ bên trong Sidebar hoặc Content. |
| **2026-10-11** | Antigravity AI + User | Tính Năng Thu Gọn / Mở Rộng Section Trong Thống Kê (`-` & `+`): Bổ sung nút chuyển đổi thông minh có biểu tượng `-` (Minus) và `+` (Plus) trên tiêu đề của cả 2 khối: **Hoạt Động Học Tập Trong Tuần** và **Tiến Độ Học Tập Từng Thành Viên**. Cố định cứng vị trí nút trên hàng tiêu đề đầu tiên (`section-header-top-row` cố định 32px và padding card 20px 24px), đảm bảo cả 2 dấu `-` và `+` nằm chính xác tại 1 vị trí cố định tuyệt đối, không bao giờ bị xịch lên xuống khi đóng mở. |
| **2026-10-12** | Antigravity AI + User | Bổ Sung Cụm Nút Đổi Ngày Trước (`<`) & Ngày Sau (`>`): Tích hợp 2 nút điều hướng nhanh ngày hôm trước và ngày hôm sau bao quanh nút chọn ngày (`DatePicker.tsx`, `useDatePicker.ts`, `DatePicker.css`). Bấm `<` lùi 1 ngày, bấm `>` tiến 1 ngày, lọc dữ liệu nhật ký học ngay lập tức và tự động đồng bộ thanh lịch tuần `CalendarStrip`; tối ưu hiển thị ngày ngắn gọn (`CN, 11/10`) trên điện thoại chống tràn màn hình; cố định chiều ngang bằng `scrollbar-gutter: stable` ngăn giật giao diện khi thanh cuộn xuất hiện. |
| **2026-10-12** | Antigravity AI + User | Khắc Phục Lỗi Lệch Múi Giờ Ngày Hiện Tại (Timezone Bug Fix): Thay thế hoàn toàn phương pháp lấy ngày cũ `new Date().toISOString().split('T')[0]` (luôn bị trả về giờ UTC khiến các múi giờ GMT+7 như Việt Nam bị lùi về ngày hôm qua vào khung giờ 00:00 - 07:00 sáng). Xây dựng module `src/utils/date.ts` với `getTodayLocalISO()`, `shiftDateLocalISO()`, `getYesterdayLocalISO()` theo giờ địa phương thực tế của người dùng, đảm bảo ứng dụng khi mở lên luôn mặc định chính xác 100% vào đúng ngày hôm nay. |
| **2026-10-12** | Antigravity AI + User | Nâng Chuẩn Breakpoint Mobile Lên 1024px (Tương Thích Mọi Dòng iPhone Pro Max & Tablet): Chuyển toàn bộ mốc phân chia Desktop/Mobile từ 900px lên **1024px** trên toàn bộ hệ thống (`App.css`, `index.css`, `TodoList.css`, `TodoInput.css`). Xử lý triệt để việc iPhone Pro Max xoay ngang (932px - 956px) hoặc Safari bật chế độ máy tính (980px) bị nhảy sang giao diện desktop; đồng thời ẩn triệt để thanh công cụ `FilterBar` trên mobile (`display: none !important`), mang lại giao diện di động chuẩn xác và hoàn mỹ 100% trên mọi dòng máy. |
| **2026-10-12** | Antigravity AI + User | Hiển Thị Tên Thành Viên Theo Ngữ Cảnh Chế Độ Xem: Khi xem chế độ **Tất Cả** (`studentFilter === 'all'`), các dòng nhật ký học tự động gắn thêm badge tên thành viên phân biệt màu sắc (`[Trường]` xanh dương, `[Nguyệt]` hồng) ở đầu dòng giúp nhận diện nhanh; khi bấm vào tab chi tiết từng thành viên (`Trường` hoặc `Nguyệt`), badge tên tự động ẩn đi để giữ giao diện tối giản, thanh thoát (`TodoItem.tsx`, `TodoList.tsx`). |
| **2026-10-12** | Antigravity AI + User | Khắc Phục Triệt Để Lỗi Vỡ Layout Desktop Trên Safari iOS (iPhone XS Max): Xử lý tận gốc nguyên nhân Safari iOS < 16.4 bỏ qua Media Query do esbuild tự chuyển sang cú pháp `@media (width<=1024px)`. Cấu hình `cssTarget: ['safari14', 'ios14']` trong `vite.config.ts` để ép esbuild giữ cú pháp chuẩn tương thích `@media (max-width: 1024px)`. Đồng thời triển khai cơ chế bảo vệ 2 lớp (Dual-Layer) với hook `useIsMobile(1024)` trong React `App.tsx` gỡ bỏ hoàn toàn (unmount) Sidebar và Desktop Header khỏi DOM trên mobile/tablet, triệt tiêu 100% rủi ro hiển thị giao diện máy tính trên thiết bị di động. |
| **2026-10-13** | Antigravity AI + User | Triển Khai Tính Năng Học Từ Vựng Flashcards & Thuật Toán Lặp Lại Ngắt Quãng (Spaced Repetition - SRS): (1) **Backend**: Xây dựng module `Flashcards` (`flashcards.controller.ts`, `flashcards.service.ts`, `flashcard.schema.ts`) cài đặt thuật toán SuperMemo-2 (SM-2) tính toán khoảng cách ngày ôn tự động theo 4 cấp độ đánh giá: Quên (`again` +1 ngày), Khó (`hard` +2 ngày), Tốt (`good` +4 ngày), Dễ (`easy` +7 ngày); tích hợp API tự động tra từ vựng (`lookup`) qua Free Dictionary API (lấy chuẩn IPA, phát âm MP3, nghĩa, ví dụ); cung cấp bộ dữ liệu hạt giống 30+ từ vựng IELTS Academic & Oxford Core chất lượng cao; (2) **Frontend**: Tích hợp Tab thứ 3 **"Từ Vựng 🎴"** trên cả Desktop Header và Mobile Header; xây dựng component `FlashcardView` với hiệu ứng lật thẻ 3D (`perspective: 1200px`), hỗ trợ phát âm tức thì qua Web Speech API / MP3, thanh tiến độ học, chế độ Kho từ vựng (Word Bank) kèm lọc theo chủ đề và tìm kiếm; bổ sung modal thêm từ mới (`FlashcardModal`) có nút tra từ tự động 1-chạm. |
| **2026-10-13** | Antigravity AI + User | Nạp Toàn Bộ Kho Từ Vựng Oxford 3000 & 5000 (4,954 Từ) & Phân Loại 13 Chủ Đề Ngữ Nghĩa Song Ngữ: (1) **Data & Database**: Tích hợp toàn bộ **4,954 từ cốt lõi & nâng cao** Oxford 3000 & Oxford 5000 vào MongoDB Atlas với đầy đủ IPA, nghĩa tiếng Việt, định nghĩa tiếng Anh, câu ví dụ và cấp độ CEFR (A1-C1); (2) **Phân Loại Chủ Đề Tự Động**: Viết script phân loại ngữ nghĩa song ngữ (`classify-topics.js`) tự động gán toàn bộ 4,954 từ vào 13 chủ đề chuẩn: Giáo dục & Học tập (1,474 từ), Công việc & Kinh doanh (685 từ), Môi trường & Thiên nhiên (663 từ), Khoa học & Công nghệ (553 từ), Con người & Mối quan hệ (452 từ), Giao tiếp & Đời sống (342 từ), Sức khỏe & Y tế (209 từ), Ẩm thực & Nấu nướng (203 từ), Du lịch & Giao thông (149 từ), Nghệ thuật & Giải trí (109 từ), Cảm xúc & Tâm lý (43 từ), Thể thao & Vui chơi (37 từ), Pháp luật & Chính trị (35 từ); (3) **Frontend Tối Ưu**: Bổ sung bộ lọc học ôn theo chủ đề (`studyTopic`) kết hợp cấp độ (`studyLevel`); tích hợp thanh lướt chọn chủ đề trực quan dạng chip (`Topic Chips Bar`) hiển thị icon + tên tiếng Việt + số lượng từ trong kho từ vựng; gắn badge chủ đề có icon trên từng thẻ 3D và thẻ từ vựng; tối ưu hiệu năng hiển thị phân trang 60 từ/mẻ nhẹ nhàng trên mobile. |
| **2026-10-13** | Antigravity AI + User | Triển Khai Hệ Thống Học Tiếng Hàn Toàn Diện 4 Giai Đoạn (Korean Learning Hub - Zero to Hero): (1) **Giai Đoạn 1 (HangeulView)**: Bảng 40 chữ cái Hangeul tương tác (10 nguyên âm cơ bản, 11 nguyên âm ghép, 14 phụ âm cơ bản, 5 phụ âm căng) có phát âm giọng Hàn `ko-KR` + Bộ mô phỏng ghép khối chữ (Syllable Builder) theo thuật toán Unicode + Mini Quiz 10 câu trắc nghiệm phản xạ nhận diện chữ; (2) **Giai Đoạn 2 (KoreanVocabView)**: Kho từ vựng TOPIK 1 theo 8 chủ đề thiết yếu + Thẻ 3D Flashcard có phân tích âm Hán-Hàn tương đương + Thuật toán ôn tập ngắt quãng SRS SM-2 (Again, Hard, Good, Easy) lưu localStorage bền vững; (3) **Giai Đoạn 3 (KoreanGrammarView)**: Sổ tay 8 chủ điểm ngữ pháp nền tảng (Trật tự câu SOV, Trợ từ `은/는`, `이/가`, `을/를`, `에/Giữa`, Đuôi câu `-아요/어요`, `-습니다`, `-고 싶다`) kèm công thức, mẹo nhớ và ví dụ có âm thanh; (4) **Giai Đoạn 4 (KoreanDialogueView)**: 60+ mẫu câu giao tiếp sống còn theo 5 tình huống + Công cụ dịch 2 chiều Hàn ⇄ Việt (Google GTX API) có nghe phát âm, copy và 1-chạm lưu thành Flashcard cá nhân; (5) **Container & Navigation**: Tích hợp `KoreanHubView` vào Header Desktop và MobileHeader, tối ưu giao diện cảm ứng không tràn màn hình. |
| **2026-10-13** | Antigravity AI + User | Nâng Cấp Toàn Diện Sư Phạm & Tính Năng Học Tiếng Hàn (Theo Bộ 10 Góp Ý Chuyên Sâu): (1) **Truy Xuất Chủ Động & Trắc Nghiệm Khách Quan (Active Recall)**: Tích hợp 4 đáp án trắc nghiệm khách quan trực tiếp trên mặt trước thẻ Flashcard trước khi lật, chống tự chấm "Good" ảo; bổ sung 3 chế độ học linh hoạt: `Hàn ➔ Việt` (Nhận diện), `Việt ➔ Hàn` (Phản xạ/Gợi nhớ), và `Nghe ➔ Chọn` (Luyện nghe); (2) **Thuật Toán SM-2 Chuẩn Xác (Authentic SuperMemo-2)**: Tính toán hệ số ghi nhớ Ease Factor (`EF` điều chỉnh từ 1.3 đến 3.0) và khoảng cách ngày ôn tăng lũy tiến khách quan; (3) **Ẩn/Hiện Phiên Âm Latin (Romanization Toggle 👁️)**: Công tắc lưu localStorage trên toàn hệ thống giúp người học cai phụ thuộc Latin, ép đọc mặt chữ Hangul; (4) **Học Patchim & Quy Tắc Biến Âm (7 Âm Đại Diện)**: Bổ sung tab bài học chuyên sâu về 7 nhóm phụ âm cuối đại diện (`ㄱ, ㄴ, ㄷ, ㄹ, ㅁ, ㅂ, ㅇ`) và 4 quy tắc biến âm cốt lõi (Nối âm Liaison, Mũi hóa, Căng âm, Bật hơi) kèm phát âm âm thanh mẫu; hiển thị dạng viết vs dạng đọc thực tế (`[함니다]`, `[머거요]`); (5) **Trình Giả Lập Bàn Phím Tiếng Hàn 2-Set (두벌식 Dubeolsik)**: Phân chia phụ âm bên trái, nguyên âm bên phải, hỗ trợ gõ phím vật lý máy tính & phím ảo chạm màn hình, hỗ trợ Shift và bộ từ mẫu thực hành (`사과`, `한국`, `학교`); (6) **Cảnh Báo Bản Dịch Máy (Machine Translation Warning Modal)**: Bổ sung popup kiểm duyệt & chỉnh sửa câu dịch trước khi lưu vào Flashcard, gắn nhãn nhận diện `[Bản dịch máy ⚠️]`; (7) **Mục Tiêu Học Ngày (Daily Goal 🔥)**: Theo dõi tiến độ 10 thẻ/ngày có lưu bền vững. |
| **2026-10-13** | Antigravity AI + User | Tái Cấu Trúc Toàn Diện Ứng Dụng Chuyên Sâu Học Tiếng Hàn (Korean-First Pivot): Loại bỏ hoàn toàn các thành phần nhật ký học tiếng Anh cũ (Tab Báo cáo `reports`, Tab Thống kê học viên `stats`, Sidebar học viên Trường/Nguyệt `StudentSidebar`, bộ lọc ngày `DatePicker`, form thêm `TodoInput`, đồng hồ bấm giờ `StudyTimer`, thanh tìm kiếm `FilterBar`). Ứng dụng biến đổi thành **TaskFlow Korean** thuần túy: mở ngay lập tức vào **Lộ Trình Học Tiếng Hàn Toàn Diện (`KoreanHubView`)**, giữ lại tab phụ Từ Vựng Tiếng Anh (`FlashcardView`), giải phóng 100% diện tích màn hình trên desktop và mobile, giảm gần một nửa dung lượng CSS/JS bundle, tốc độ phản hồi tức thì 0ms. |
| **2026-10-13** | Antigravity AI + User | Triển Khai Toàn Diện Hạng Mục P0 Theo Góp Ý Của AI Reviewer: (1) **Bộ gõ IME Automata 2-Set (HangulImeAutomata)**: Ghép âm tiết động Unicode thời gian thực, xử lý phụ âm cuối nhảy sang âm sau khi gặp nguyên âm (`먹` + `ㅓ` ➔ `머거`), ghép patchim kép/nguyên âm phức, backspace từng nét Jamo, ánh xạ phím vật lý `event.code` triệt tiêu xung đột Telex; (2) **Mở rộng kho từ vựng lên 124 từ TOPIK 1**: Bổ sung từ loại POS, dạng từ điển & dạng chia (-아요/어요), phân loại gốc Hán (đồng nghĩa, bạn giả, thuần Hàn), cảnh báo bạn giả (`인사`, `방학`, `공부`, `기분`, `약속`); (3) **16 Chủ điểm ngữ pháp + Bài tập trắc nghiệm**: Bổ sung `이다`, `아니다`, `있다/없다`, `이/그/저`, phủ định `안`, quá khứ `-았/었어요`, `-(으)세요/-주세요`, số đếm & đơn vị đếm; mỗi bài có bài tập điền khuyết tự động chấm; (4) **Lợi thế người Việt & Cặp âm tối thiểu (Minimal Pairs)**: So sánh patchim với -c, -t, -p, -m, -n, -ng; luyện bộ ba phụ âm thường/bật hơi/căng (`불/풀/뿔`, `자다/차다/짜다`...) và nguyên âm dễ nhầm (`ㅓ/ㅗ`, `ㅡ/ㅜ`); (5) **SRS Nâng Cao**: In-session learning steps (lặp lại thẻ sai sau 3-4 thẻ trong phiên), phát hiện thẻ Leech ($\ge 4$ lần sai), ghi log ôn tập (`ReviewLog`), chế độ gõ từ vựng (Type-In Recall). |
| **2026-10-14** | Antigravity AI + User | Đổi Mới Sư Phạm Hangeul Cho Người Việt Từ Số 0 (Lộ Trình 5 Ngày Vỡ Lòng & Viết Tên Hangeul): (1) **Tái Cấu Trúc Lộ Trình 5 Ngày Nhẹ Nhàng**: Thay thế bảng 40 chữ cái choáng ngợp bằng lộ trình 5 ngày chia nhỏ (5-7 chữ/ngày) gắn liền với 5-8 từ vựng thật có nghĩa ngay trong ngày: Ngày 1 (3 từ đầu tiên `아이`, `오이`, `우유` với 6 nguyên âm + 3 phụ âm nòng cốt), Ngày 2 (7 phụ âm phát âm & đồ vật quen thuộc), Ngày 3 (Nguyên lý thêm nét vua Sejong `ㄱ➔ㅋ`, `ㄷ➔ㅌ`, `ㅂ➔ㅍ` + Cơn mưa Dopamine từ Konglish `커피`, `피자`, `카메라`), Ngày 4 (Phụ âm căng & Từ vựng gia đình), Ngày 5 (Bộ đánh vần Patchim tiếng Việt `-c, -t, -p, -m, -n, -ng` ➔ đọc thông `안녕하세요!`); (2) **Quy Trình Học 3 Bước Mỗi Ngày**: Bước 1 (Âm trước - hình sau kèm mẹo nhớ hình tượng & phát âm giọng bản xứ), Bước 2 (Đọc từ vựng thật ngoài đời kèm phân tích cấu tạo chữ), Bước 3 (Thử thách phản xạ nhanh có chấm điểm & giải thích sư phạm), Bước 4 (Huy hiệu chiến thắng mở ngày tiếp theo); (3) **Tự Viết Tên Bạn Bằng Hangeul ✍️**: Danh bạ 50+ tên tiếng Việt thông dụng phiên âm chuẩn Hán-Hàn kèm ý nghĩa và phát âm giọng đọc, hỗ trợ tìm kiếm không dấu thời gian thực và sao chép thiệp tên (Name Card) chia sẻ; (4) **Mobile Responsive Hoàn Hảo**: Thanh chọn ngày vuốt ngang cảm ứng mượt mà, layout thẻ chữ cái và từ vựng co giãn linh hoạt, không tràn ngang màn hình. |
| **2026-10-14** | Antigravity AI + User | Tối Ưu Không Gian Với Menu Dropdown Ngoài Header & Bổ Sung Quy Tắc Mobile Responsive Tiên Quyết: (1) **Chuyển Đổi Thành Menu Ngoài (Outside Header Dropdown)**: Xóa bỏ hoàn toàn thanh 4 nút bấm to ngang gây tốn diện tích trong nội dung trang (`kr-stage-tabs-row`); đưa 4 giai đoạn lên thanh Header ngoài cùng dạng nút bấm tinh gọn `[🔤 GĐ 1: Bảng Chữ Cái ▾]`. Click vào sẽ xổ ra menu kính mờ tuyệt đẹp hiển thị chi tiết 4 giai đoạn kèm badge tóm tắt và mô tả nội dung; (2) **Giải Phóng Diện Tích Tối Đa**: Màn hình học bên dưới lập tức hiển thị nội dung phân hệ ngay trên cùng, tăng hơn 140px diện tích hiển thị dọc; (3) **Chuẩn Hóa Quy Tắc Doc Mobile Responsive**: Bổ sung điều khoản bắt buộc và tiên quyết vào `AGENTS.md` và `.agents/rules/project-rules.md`: Mọi tính năng/thành phần UI đều phải responsive 100% trên màn hình di động (< 1024px, < 768px, < 480px, < 360px), tuyệt đối không tràn màn hình ngang (Zero Horizontal Scroll), tối ưu kích thước chạm cảm ứng $\ge 36\text{px} - 44\text{px}$ và popover/dropdown tự co giãn an toàn. |
| **2026-10-14** | Antigravity AI + User | Triển Khai Lộ Trình Học Từ Vựng Hàng Ngày 30 Ngày (30-Day Daily Korean Vocab Roadmap): (1) **Giáo Trình 30 Ngày Sư Phạm Chặt Chẽ (240 Từ Vựng)**: Phân bổ khoa học qua 5 tuần TOPIK 1 (Tuần 1: Sinh tồn & Cá nhân, Tuần 2: Động từ hành động đời sống, Tuần 3: Tính từ & Miêu tả cảm xúc/thời tiết, Tuần 4: Số đếm, Lịch trình, Cơ thể & Mua sắm, Chặng cuối: Liên từ nối câu & Về đích TOPIK 1); (2) **Cấu Trúc Từng Ngày Học**: Mỗi ngày gồm 8 từ vựng kèm Mục tiêu bài học (Focus), Mẹo ngữ pháp đi kèm (Grammar Tip) và Lời khuyên ghi nhớ thực chiến (Quick Advice); (3) **Thẻ Từ Vựng Đa Tầng**: Hangeul lớn, audio phát âm giọng đọc `ko-KR`, phiên âm Latin (hỗ trợ toggle ẩn/hiện), phát âm thực tế biến âm, liên kết âm Hán - Việt tương đương và câu ví dụ song ngữ Hàn - Việt thực tế; (4) **Tích Hợp Thẳng Vào Flashcard SRS & Active Recall**: Nút `Bắt đầu học Flashcard Ngày X` 1-chạm nạp ngay 8 từ của ngày vào vòng lặp ôn tập 3D Flashcard & Trắc nghiệm phản xạ, nút `Đánh dấu đã học xong` lưu trạng thái bền vững vào `localStorage` kèm thanh tiến độ 30 ngày (0-100%); (5) **Thẻ Xem Trước Ngày Mai (Tomorrow's Preview)**: Cho phép người học xem lướt từ vựng và chủ đề của ngày kế tiếp giúp tạo động lực học tập liên tục; (6) **Tối Ưu Mobile 100%**: Thiết kế thanh chọn ngày trượt ngang cảm ứng, nút bấm lớn $\ge 40\text{px}$, chống tràn ngang màn hình tuyệt đối trên mọi kích cỡ điện thoại. |
| **2026-10-14** | Antigravity AI + User | Tái Thiết Kế Footer Cao Cấp & Xử Lý Triệt Để Lỗi Cuộn / Dính Mép Màn Hình (Footer Redesign & Sticky Scroll Fix): (1) **Khắc Phục Lỗi Dính Mép Màn Hình (Sticky / Cutoff Bug)**: Gỡ bỏ quy tắc cũ `overflow: hidden; height: 100%` trên desktop trong `index.css`, chuyển sang `html, body { overflow-y: auto; scroll-behavior: smooth }` cho phép cuộn trang tự nhiên mượt mà trên cả desktop và mobile; tăng khoảng cách đệm đáy `padding-bottom: 48px - 56px` cho `.app-layout-wrapper`; bổ sung margin cho banner mẹo sư phạm trong `HangeulView`; (2) **Component Footer Chuyên Biệt (`Footer.tsx` & `Footer.css`)**: Thay thế thẻ footer đơn sơ cũ bằng giao diện kính mờ Glassmorphism cao cấp gồm 3 phần: Phần đầu (Logo Hangeul `한`, tên thương hiệu TaskFlow Korean, version tag `v1.0.14`, nút Cuộn lên đầu trang mượt mà); Phần thân (Lối tắt chuyển nhanh 4 giai đoạn học có đánh dấu `Đang học`, bộ chip tính năng trọng tâm SM-2/IME/TTS/Romaja, hộp châm ngôn tiếng Hàn cổ vũ `"천 리 길도 한 걸음부터"`); Phần đáy (Bản quyền, thông tin Local-First & AWS CloudFront Live, huy hiệu 100% Mobile Responsive). |
| **2026-10-15** | Antigravity AI + User | Tối Ưu Toàn Diện Giao Diện Mobile & Bộ 6 Đột Phá Tính Năng Mới Cho TaskFlow Korean: (1) **Tối Ưu Mobile Toàn Diện (Mobile-First UX Transformation)**: Xây dựng thanh điều hướng đáy màn hình di động `MobileBottomNav` chuẩn iOS/Android hỗ trợ safe area, chuyển 1-chạm 4 giai đoạn bằng ngón cái; tinh giản Header mobile thành 1 hàng duy nhất (~50px), ẩn dropdown cồng kềnh, giải phóng hơn 110px chiều cao; hạ chiều cao thẻ Flashcard trên mobile từ 420px xuống 260px-270px giúp hiển thị trọn vẹn thẻ + 4 nút chấm điểm SM-2/trắc nghiệm trên 1 màn hình duy nhất không cần cuộn trang; tăng đệm an toàn `padding-bottom: 76px + safe-area`; (2) **Cây Phả Hệ Âm Hán-Hàn ("Học 1 Biết 10" - `SinoKoreanView`)**: Tra cứu 12+ gốc Hán-Hàn cốt lõi (`학`, `생`, `인`, `국`, `원`, `식`, `화`, `전`, `동`, `실`, `문`, `기`), bung ra mạng lưới từ ghép phái sinh, phát âm và câu ví dụ; kèm Mini Game ghép 2 gốc chữ sinh từ có tính điểm; (3) **Trình Lắp Ráp Câu SOV & Luyện Trợ Từ (`SentenceBuilderView`)**: 10+ câu tình huống đời sống luyện trật tự câu SOV (Chủ - Nơi chốn - Tân ngữ - Động từ) và phân biệt trợ từ `-은/는`, `-이/가`, `-을/를`, `-에/에서`, có giải thích sư phạm tức thì; (4) **Bảng Tra Cứu & Luyện Chia Động Từ (`VerbConjugatorView`)**: Tra cứu 18+ động từ thông dụng, hiển thị 7 thể chia (hiện tại thân mật, kính ngữ, quá khứ, tương lai, phủ định, mong muốn, kính trọng), phân tích bất quy tắc ㅂ, ㄷ, ㅡ, 르; kèm Mini Drill 60s luyện chia nhanh; (5) **Hội Thoại Nhập Vai 2 Chiều (`RoleplayView`) & Nhận Diện Giọng Nói Micro (Web Speech Recognition)**: 3 kịch bản thực chiến (Mega Coffee, Cửa hàng CU, Đi Taxi Seoul) với lời thoại NPC, audio và phân nhánh câu trả lời; tích hợp Mic nhận diện giọng nói `ko-KR` chấm điểm độ chính xác phát âm; (6) **Chuỗi Ngày Học & Nhiệm Vụ Hàng Ngày (`DailyQuestsModal` & Heatmap)**: Bảng theo dõi Streak ngọn lửa 🔥, 3 nhiệm vụ hàng ngày có thanh tiến độ và ma trận chăm chỉ 14 ngày (14-Day Heatmap). |
| **2026-10-15** | Antigravity AI + User | Thu Gọn Triệt Để Chiều Cao Cụm Header Trên Mobile (Tiết Kiệm >200px Diện Tích Dọc): (1) **Top Header (`Header.css`)**: Tinh gọn `app-top-header` còn ~36px trên mobile ($\le 900$px), thu nhỏ logo 24px, ẩn tag `Korean` dài trên phone (giữ `TaskFlow`), ẩn text tab `Tiếng Hàn`/`Tiếng Anh` giữ lại icon 🇰🇷 và 🎴, streak pill gọn nhẹ; (2) **Thanh Chế Độ Hangeul (`.hangeul-nav-bar`)**: Gỡ bỏ khung card có viền/đệm nặng nề, chuyển thành 1 hàng phẳng ngang duy nhất (chiều cao 28px); subtabs cuộn vuốt ngang mượt mà, nút toggle phiên âm Latin hiển thị icon mắt `[👁️]` inline siêu gọn gàng; (3) **Thanh Chọn Lộ Trình 5 Ngày (`.journey-days-selector-bar`)**: Ẩn phụ đề dài 2 dòng trên mobile, biến 5 ngày thành cụm pill phẳng 1 dòng `[Ngày 1]` `[Ngày 2]`... (cao 28px); (4) **Header Ngày Học (`.journey-current-header-card`)**: Ẩn badge tên dài trùng lặp và đoạn mô tả 2 dòng, tích hợp 3 bước học thành thanh Segmented Control (`1. Chữ cái (9)` \| `2. Từ thật (7)` \| `3. Thử thách ⚡`) cao 28px; (5) **Đồng Bộ Cụm Toolbar Các Giai Đoạn**: Áp dụng layout thanh công cụ phẳng 1 dòng cho Từ Vựng TOPIK 1, Ngữ Pháp và Hội Thoại; giảm padding `App.css` và khoảng cách gap xuống 6px, đưa nội dung bài học/chữ cái lên ngay đỉnh tầm mắt của người học. |
| **2026-10-15** | Antigravity AI + User | Triển Khai Chuyên Mục Cẩm Nang Bí Quyết & Động Lực Học Tiếng Hàn (`StudyAdviceModal` & 16 Lời Khuyên Sư Phạm Thực Chiến): (1) **Bộ 16 Lời Khuyên Sư Phạm & Châm Ngôn Truyền Lửa (`studyAdviceData.ts`)**: Phân bổ theo 4 chuyên đề cốt lõi: Tâm Lý & Động Lực (Quy tắc 15 phút vàng, giải mã cơ chế quên sinh học, vượt qua nỗi sợ nói sai, cơn mưa Dopamine), Chữ Cái & Phát Âm (Lợi thế Patchim của người Việt, cai Romaja, mẹo hình học Âm-Dương, thử thách tờ giấy ăn), Gốc Hán & Từ Vựng (Mỏ vàng Hán-Hàn "học 1 biết 10", Active Recall 3 giây, học cụm Collocation, quy tắc 24h vàng), Ngữ Pháp & Phản Xạ (Tư duy đảo ngược SOV, bí kíp phân biệt `-은/는` vs `-이/가`, kỹ thuật Shadowing nhại giọng, tự độc thoại phòng ngủ thành Seoul); (2) **Giao Diện Modal Kính Mờ Cao Cấp (`StudyAdviceModal.tsx` & `.css`)**: Thẻ Spotlight "Lời khuyên hôm nay" có nút đổi ngẫu nhiên 🎲, phát âm châm ngôn tiếng Hàn bằng TTS `ko-KR`, thanh tiến độ làm chủ thói quen (`localStorage`), thanh lọc danh mục chip vuốt ngang và ô tìm kiếm từ khóa thời gian thực; nút toggle đánh dấu "Đã áp dụng thói quen này ✅" và nút 1-chạm sao chép chia sẻ; (3) **Tích Hợp Khắp Ứng Dụng**: Nút bấm `💡 Lời Khuyên` nổi bật phát sáng vàng hổ phách trên Desktop Header và icon tròn `💡` siêu gọn trên Mobile Header; nút mở cẩm nang trong hộp Châm Ngôn Học Tập tại Footer. |
| **2026-10-16** | Antigravity AI + User | Chuẩn Hóa Văn Phong & Loại Bỏ Triệt Để Từ Ngữ Không Phù Hợp / Ít Dùng: (1) **Loại bỏ từ "vỡ lòng"**: Chuyển "Lộ Trình 5 Ngày (Vỡ Lòng)" thành **"Lộ Trình 5 Ngày 🚀"** ngắn gọn, hiện đại và chuẩn xác trên toàn bộ thanh điều hướng, code và dữ liệu; (2) **Thanh lọc từ ngữ cổ / ít dùng**: Thay thế "3 từ đầu đời" thành "9 Ký Tự Nền Tảng & 7 Từ Vựng Đầu Tiên", "âm căng đanh" thành "phụ âm căng", "câu chào thần thánh" thành "câu chào thông dụng nhất", "tờ giấy thần kỳ" thành "mẹo nhỏ với tờ giấy mỏng", "nhại giọng" thành "đọc đuổi", "cơn mưa dopamine" thành "nuôi dưỡng động lực"; giữ văn phong sư phạm trong sáng, tự nhiên, văn minh và gần gũi với người học tiếng Hàn Việt Nam. |
| **2026-10-16** | Antigravity AI + User | Tái Cấu Trúc Đa Thư Mục Git & Workspace Độc Lập (Multi-Root Workspace & Isolated Git Architecture): (1) **Gom Mã Nguồn Vào `source/`**: Di chuyển trọn vẹn `frontend/` và `backend/` vào thư mục `source/` (`source/frontend/` và `source/backend/`); (2) **Git Ignore Cha Cho `source/`**: Cấu hình `.gitignore` tại root bỏ qua hoàn toàn `source/`, đảm bảo 2 Git repository con (`todo-fe` và `todo-be`) giữ nguyên tính độc lập 100%, không bị can thiệp chéo hay đè lên nhau; (3) **Git Quản Lý Tài Liệu & Agent**: Khởi tạo Git repository cha tại root quản lý `.agents/`, `docs/`, `AGENTS.md`, các file đặc tả và cấu hình hạ tầng; (4) **Multi-Root Workspace (`taskflow.code-workspace`)**: Tạo file workspace chuẩn VS Code hiển thị đồng thời cả 3 Git repos trên Explorer và Source Control panel của IDE (Docs & Agents, Frontend React TS, Backend NestJS). |
| **2026-10-16** | Antigravity AI + User | Nâng Cấp Giao Diện Tone Hồng K-Pastel Blossom & 2 Tính Năng Sáng Tạo (K-Pop/K-Drama Lyrics Decoder & Uốn Lưỡi Siêu Tốc): (1) **Theme Switcher & K-Pastel Blossom UI**: Tái thiết kế toàn bộ hệ thống màu sắc mặc định sang tone hồng hoa anh đào sáng dịu mát (Pastel Blossom `#fff0f3`), nền radial gradient ngọt ngào, card kính mờ hồng sữa và tương phản cao chữ tím than sẫm `#1f1635`; tích hợp nút toggle 1-chạm 🌸/🌙 trên Header; (2) **Giải Mã Lời Bài Hát K-Pop & Phim K-Drama (`KDramaQuotesView`)**: Tuyển tập 12 bản hit K-Pop (BTS, BLACKPINK, NewJeans, IU, Taeyeon, IVE) & phim kinh điển (Queen of Tears, Hạ Cánh Nơi Anh, Goblin, Reply 1988), phát âm giọng Hàn `ko-KR`, bóc tách từng từ vựng, ngữ pháp trọng tâm và nút 1-chạm lưu vào Flashcard SRS; (3) **Thử Thách Uốn Lưỡi Siêu Tốc (`TongueTwisterView` 잰말놀이)**: 6 câu líu lưỡi từ Cấp 1 đến Trùm Cuối đài KBS, 3 tốc độ phát âm (Rùa 0.75x, Thỏ 1.0x, Báo Đốm 1.35x), tích hợp Micro Web Speech chấm điểm độ chính xác % và trao huy hiệu chinh phục; (4) **100% Mobile Responsive**: Tối ưu chuẩn ngón tay cái, tab trượt ngang 0 horizontal scroll trên mọi điện thoại. |
| **2026-10-16** | Antigravity AI + User | Tinh Chỉnh Toàn Diện Độ Tương Phản & Thẩm Mỹ Giao Diện Pastel Blossom (UI Color & Contrast Harmony): (1) **Khắc Phục Lỗi Ký Tự Trắng Ẩn Trên Nền Trắng**: Thay thế triệt để `color: #fff;` trên toàn bộ thẻ chữ cái Hangeul (`.journey-letter-char`, `.hangeul-char-big`, `.word-hangeul-text`) bằng `color: var(--text-main);` (`#1e1b4b` Deep Indigo Charcoal sắc nét trên nền sáng và sáng dịu trên dark theme), xử lý triệt để hiện tượng chữ cái tàng hình không đọc được; (2) **Xóa Bỏ Khối Slate Tối Mù Giữa Trang (`.journey-current-header-card`)**: Chuyển đổi khối tổng quan ngày học từ nền slate tối (`rgba(30, 41, 59)`) sang card kính mờ hoa anh đào thanh nhã (`linear-gradient(135deg, rgba(255, 255, 255, 0.98), rgba(255, 241, 245, 0.95))`), viền hồng ngọc mềm mại và đổ bóng cao cấp; (3) **Hài Hòa Hệ Thống Badge**: Loại bỏ màu xanh lơ (cyan `#38bdf8`) bị lạc tông, chuyển sang tông hồng ngọc (`#e11d48`) và cam mật ong (`#d97706`) ấm áp, chuẩn thẩm mỹ K-Design; (4) **Chuẩn Hóa Danh Xưng "5 Bước Nền Tảng"**: Đổi tên toàn bộ "Lộ Trình 5 Ngày" thành **"5 Bước Nền Tảng ⭐"** và các pill ngày thành **"Bước 1, Bước 2, Bước 3, Bước 4, Bước 5"**, tạo cảm giác nhẹ nhàng, học tập theo từng cột mốc tự nhiên không gò bó thời gian; (5) **Kiểm Tra Build 0 Lỗi**: Xác nhận biên dịch Vite sạch sẽ (`npm run build`), 0 lỗi TypeScript. |









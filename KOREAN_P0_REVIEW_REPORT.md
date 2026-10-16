# 🇰🇷 Báo Cáo Triển Khai Tính Năng Học Tiếng Hàn (P0 Roadmap Implementation Report)
*Dành cho AI Chuyên Gia / Ban Cố Vấn Ngôn Ngữ & Kỹ Thuật Review Tiếp Tục*

---

## 📌 Tóm Tắt Nhanh (Executive Summary)
Dựa trên bản nhận xét chuyên sâu từ bạn, hệ thống **TaskFlow Korean** đã hoàn thành toàn bộ hạng mục **P0 (Ưu tiên cao nhất)** và giải quyết dứt điểm 3 vấn đề lớn nhất được chỉ ra:

1. **Kho từ vựng mở rộng từ 30 lên 124 từ TOPIK 1 cốt lõi**:
   - Schema bổ sung: Từ loại (`partOfSpeech`), Dạng từ điển (`dictionaryForm`), Dạng chia thân mật (`conjugatedForm`), Gốc Hán-Hàn (`hanjaVi`), Phân loại quan hệ Hán-Việt (`hanjaType`: đồng nghĩa, bạn giả, thuần Hàn), Cảnh báo bạn giả (`hanjaWarning`), Phát âm thực tế (`actualPronunciation`).
2. **Bộ gõ tiếng Hàn IME Automata ghép khối chuẩn (Dubeolsik 2-Set IME)**:
   - Xây dựng hoàn chỉnh thuật toán máy trạng thái `{cho, jung, jong}` ghép âm tiết Unicode thời gian thực.
   - Xử lý mượt mà:
     - Ghép phụ âm + nguyên âm thành khối vuông (`ㄱ` + `ㅏ` ➔ `가`).
     - Tự động nhảy phụ âm cuối sang âm sau khi gặp nguyên âm (Liaison: `먹` + `ㅓ` ➔ `머` + `거`).
     - Ghép nguyên âm phức (`ㅗ` + `ㅏ` ➔ `ㅘ`) và patchim kép (`ㄹ` + `ㄱ` ➔ `ㄺ`, `ㅂ` + `ㅅ` ➔ `ㅄ` như `앉다`, `없다`).
     - Phím Backspace tháo gỡ từng nét Jamo thay vì xóa cụt cả khối chữ.
     - Ánh xạ `event.code` (vật lý: `KeyQ`, `KeyR`...) triệt tiêu xung đột với bộ gõ Telex tiếng Việt hoặc IME máy tính.
3. **Mở rộng Ngữ pháp từ 8 lên 16 chủ điểm sơ cấp toàn diện + Bộ bài tập trắc nghiệm tương tác**:
   - Bổ sung đầy đủ các chủ điểm nền tảng bị thiếu: `이다` (`이에요/예요`), `아니다` (`이/가 아니에요`), `있다/없다`, đại từ chỉ định `이/그/저`, phủ định `안` & `-지 않다`, thì quá khứ `-았/었어요`, yêu cầu lịch sự `-(으)세요 / -주세요`, hệ thống số đếm Hán-Hàn vs Thuần Hàn kèm đơn vị đếm (`개, 명, 잔, 병, 마리, 권...`).
   - Mỗi chủ điểm đều có **bộ bài tập điền khuyết / trắc nghiệm phản xạ tự động chấm** kèm giải thích ngữ pháp và mẹo nhớ quy tắc.
4. **Khai thác thế mạnh của người Việt & Bộ luyện Cặp âm tối thiểu (Minimal Pairs)**:
   - Tích hợp bảng so sánh Patchim tiếng Hàn với 6 phụ âm cuối tiếng Việt: `ㄱ ≈ -c`, `ㄴ ≈ -n`, `ㄷ ≈ -t`, `ㅁ ≈ -m`, `ㅂ ≈ -p`, `ㅇ ≈ -ng` (chỉ duy nhất `ㄹ` là không có tương đương).
   - Xây dựng phân hệ **Cặp Âm Tối Thiểu (Minimal Pairs)** với phát âm audio tức thì:
     - Bộ ba phụ âm Thường vs Bật hơi vs Căng: `불/풀/뿔`, `자다/차다/짜다`, `고기/코/꼬리`, `달/탈/딸`, `방/빵`.
     - Cặp nguyên âm dễ nhầm: `ㅓ/ㅗ`, `ㅡ/ㅜ`, `ㅐ/ㅔ`.
5. **Cải tiến Thuật toán SRS & Trải nghiệm học**:
   - **In-session Learning Steps**: Khi trả lời sai hoặc chọn "Again", thẻ không bị đẩy sang ngày mai mà được tự động đưa trở lại hàng đợi sau 3-4 thẻ trong cùng phiên cho đến khi học viên trả lời đúng.
   - **Phát hiện thẻ khó (Leech Detection)**: Thẻ sai $\ge 4$ lần được gắn cờ `🚨 THẺ HAY QUÊN (LEECH)`, hiển thị cảnh báo đặc biệt, âm Hán-Hàn và câu ví dụ để người học tập trung ghi nhớ.
   - **Ghi log lượt ôn (`ReviewLog`)**: Tự động lưu lịch sử từng lượt ôn tập (`cardId`, `timestamp`, `mode`, `correct`, `rating`, `responseMs`) vào `localStorage` (`korean_review_logs`) để sẵn sàng chuyển đổi sang FSRS (`ts-fsrs`) khi có dữ liệu người dùng thật.
   - **Thêm chế độ Type-In Recall**: Nhìn nghĩa tiếng Việt ➔ gõ trực tiếp chữ tiếng Hàn bằng bàn phím IME nội bộ và tự động chấm đúng/sai.
6. **Tối ưu Mobile UI**:
   - 100% responsive, không có thanh cuộn ngang, touch targets $\ge 44\text{px}$, nhãn phím rõ ràng không chỉ dựa vào màu sắc.

---

## 🏗️ Chi Tiết Triển Khai Kỹ Thuật

### 1. Bàn Phím IME Ghép Khối (`frontend/src/utils/hangulIme.ts`)
- **Kiến trúc Automata**:
  ```typescript
  export class HangulImeAutomata {
    private completedText: string = '';
    private currentSyllable: HangulBufferState = { cho: '', jung: '', jong: '' };
    ...
  }
  ```
- **Xử lý nhảy Patchim (Liaison Consonant Shift)**:
  - Khi buffer đang có `{ cho: 'ㅁ', jung: 'ㅓ', jong: 'ㄱ' }` (chữ `먹`), nếu nhận thêm nguyên âm `ㅓ`:
    - Patchim đơn `ㄱ` lập tức chuyển thành `cho` của âm sau: `{ cho: 'ㄱ', jung: 'ㅓ', jong: '' }`.
    - Âm trước chốt lại thành `머`. Kết quả hiển thị: `머거`.
  - Khi buffer có patchim kép (ví dụ `닭` `{cho: 'ㄷ', jung: 'ㅏ', jong: 'ㄺ'}`), nếu gõ thêm nguyên âm `ㅣ`:
    - Tách `ㄺ` thành `ㄹ` và `ㄱ`.
    - Âm trước giữ `달`, âm sau nhận `기`. Kết quả hiển thị: `달기`.
- **Backspace tháo từng nét (Jamo-by-Jamo)**:
  - Patchim kép `ㄳ` ➔ lùi về `ㄱ` ➔ lùi về nguyên âm ➔ lùi về phụ âm đầu. Người học không bao giờ bị ức chế vì mất cả khối chữ.
- **Physical Key Mapping (`PHYSICAL_KEY_MAP`)**:
  - Dùng `e.code` (`KeyQ`, `KeyW`, `KeyE`...) map trực tiếp sang Jamo chuẩn Dubeolsik, ngăn chặn hoàn toàn việc Unikey/EVKey hay Telex nuốt phím.

---

### 2. Schema Từ Vựng & Phân Loại Hán-Hàn (`frontend/src/types/korean.ts` & `koreanData.ts`)
- **Đầy đủ 124 từ TOPIK 1 chia đều 8 chủ đề**:
  - Chào hỏi & Xã giao (`greetings`)
  - Số đếm & Thời gian (`numbers`)
  - Ẩm thực & Nhà hàng (`food`)
  - Gia đình & Con người (`family`)
  - Đồ vật & Đời sống (`daily`)
  - Trường học & Công việc (`school_work`)
  - Địa điểm & Mua sắm (`places`)
  - Tính từ & Cảm xúc (`feelings`)
- **Phân loại Hanja quan hệ với tiếng Việt (`HanjaRelationType`)**:
  1. `identical` (Đồng nghĩa / Đồng âm tương đồng): Ví dụ `학생` (Học sinh), `선생님` (Tiên sinh), `한국` (Hàn Quốc), `병원` (Bệnh viện), `도서관` (Đồ thư quán / Thư viện), `시간` (Thời gian)...
  2. `false_friend` (Bạn giả nguy hiểm):
     - `인사` (Nhân sự) ➔ Cảnh báo: Nghĩa là "Lời chào", không phải nhân sự!
     - `방학` (Phóng học) ➔ Cảnh báo: Là "Kỳ nghỉ hè/đông", không phải tan học về nhà!
     - `공부` (Công phu) ➔ Cảnh báo: Là "Học bài", không phải võ thuật/công phu!
     - `기분` (Khí phận) ➔ Cảnh báo: Là "Tâm trạng / Cảm xúc"!
     - `약속` (Ước thúc) ➔ Cảnh báo: Là "Cuộc hẹn / Lời hứa", không mang nghĩa tiêu cực!
     - `식사` (Thực sự) ➔ Cảnh báo: Là "Bữa ăn / Dùng bữa"!
  3. `pure_korean` (Thuần Hàn):
     - `밥` (Cơm), `물` (Nước), `집` (Nhà), `눈` (Mắt/Tuyết), `손` (Tay), `발` (Chân), `비` (Mưa)...
- **Dạng chia động từ/tính từ**:
  - Ghi rõ `dictionaryForm` (`가다`, `먹다`, `좋다`...) và `conjugatedForm` (`가요`, `먹어요`, `좋아요`...).

---

### 3. Phân Hệ Ngữ Pháp 16 Bài & Bài Tập Trắc Nghiệm (`frontend/src/components/KoreanHub/KoreanGrammarView/`)
Bao gồm toàn bộ 16 chủ điểm sơ cấp:
1. Trật tự câu SOV (S - O - V)
2. Động từ "Là": `이다` (`이에요 / 예요`)
3. Động từ "Không phải là": `아니다` (`이/가 아니에요`)
4. Trợ từ Chủ đề: `은 / 는`
5. Trợ từ Chủ ngữ: `이 / 가`
6. Trợ từ Tân ngữ: `을 / 를`
7. Sự tồn tại / Sở hữu: `있다 / 없다`
8. Trợ từ Nơi chốn & Thời gian: `에 / 에서`
9. Đại từ chỉ định: `이 / 그 / 저`
10. Đuôi câu thân mật lịch sự: `-아요 / -어요 / -여요`
11. Phủ định: `안` + V/A & `-지 않다`
12. Đuôi câu trang trọng: `-ㅂ니다 / -습니다`
13. Thì quá khứ: `-았/었어요`
14. Đề nghị / Yêu cầu lịch sự: `-(으)세요` & `-아/어 주세요`
15. Đuôi câu Mong muốn: `-고 싶다`
16. Số đếm Hán-Hàn vs Thuần Hàn & Đơn vị đếm (`개, 명, 잔, 병, 마리, 권...`)

**Mỗi bài đều tích hợp bài tập thực hành tương tác**:
- Câu hỏi điền vào chỗ trống (`blankTarget`).
- Bộ đáp án 4 lựa chọn.
- Phản hồi tức thì: Nếu đúng hiện giải thích chi tiết, nếu sai hiện gợi ý quy tắc ngữ pháp (`ruleHint`).
- Ô tìm kiếm ngữ pháp nhanh theo từ khóa.

---

### 4. Spaced Repetition SRS (SM-2 / In-Session Learning Steps)
- **Đo lường phản hồi**: Tự động đo thời gian phản hồi `responseMs` của người học và ghi vào `ReviewLog`.
- **In-Session Learning Steps**: Thẻ trả lời sai được tái chèn vào vị trí cách đó 3-4 thẻ. Người học phải làm đúng ít nhất một lần trước khi hoàn tất phiên học.
- **Leech Detection**: Đếm số lần sai lũy kế. Nếu `leechCount >= 4`, thẻ chuyển trạng thái Leech và hiển thị cảnh báo đỏ trên giao diện.
- **4 Chế độ ôn tập Active Recall**:
  1. `ko-to-vi`: Nhận diện chữ Hàn ➔ Chọn nghĩa tiếng Việt.
  2. `vi-to-ko`: Đọc nghĩa tiếng Việt ➔ Chọn chữ Hangeul đúng.
  3. `listen-to-ko`: Nghe phát âm ➔ Chọn chữ Hangeul đúng.
  4. `type-in`: Đọc nghĩa tiếng Việt ➔ Gõ chữ Hangeul bằng bàn phím và tự động chấm điểm.

---

## ❓ Câu Hỏi Xin Ý Kiến Đánh Giá (Dành Cho AI Reviewer)

1. **Về kho từ vựng (124 từ hiện tại)**:
   - Các từ vựng TOPIK 1 hiện tại đã đủ đa dạng và cân đối về từ loại (danh từ, động từ, tính từ, đại từ, phó từ, số từ) để phục vụ giai đoạn Zero-to-Hero chưa? Có từ nào cần bổ sung gấp trong Top 200 không?
2. **Về thuật toán IME Automata**:
   - Khả năng xử lý chuyển Patchim (`먹` + `ㅓ` ➔ `머거`) và tháo nét Backspace hiện tại đã giải quyết triệt để vấn đề gõ tiếng Hàn trên Web chưa? Có trường hợp góc (edge case) nào về patchim kép phức tạp (như `밟다`, `넓다`) cần lưu ý thêm không?
3. **Về bài tập ngữ pháp**:
   - Bộ 16 bài tập điền khuyết hiện tại có nên bổ sung thêm dạng bài "Sắp xếp lại câu theo trật tự SOV" hay "Chia đuôi câu động từ (-아요/어요)" dưới dạng kéo thả hoặc gõ phím không?
4. **Về giai đoạn P1 tiếp theo**:
   - Theo bạn, nên ưu tiên triển khai tính năng nào trước:
     - (A) Shadowing ghi âm tự so sánh (`MediaRecorder`) phát cạnh audio gốc?
     - (B) Checkpoint bài test xếp lớp đầu vào (Onboarding Test) cho người đã biết chữ cái?
     - (C) Chuyển đổi chính thức sang thuật toán `ts-fsrs` (Free Spaced Repetition Scheduler)?

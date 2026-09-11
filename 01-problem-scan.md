# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Lưu ý về số liệu:** Các con số thời gian/khối lượng trong file này là **ước tính giả định** dựa trên hiểu biết chung về quy trình, **chưa được đo trên dữ liệu thực tế** của Vingroup. Chúng cần được xác minh bằng phỏng vấn nhân viên vận hành và log hệ thống trước khi dùng để ra quyết định.

---

# 🔍 Phase 1 — SCAN

Quét vận hành các công ty thành viên bằng 4 lenses: **Lặp lại · Tốn thời gian · AI có thể tốt hơn · Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Khách đặt lịch sửa chữa mô tả lỗi bằng tiếng Việt tự do (*"đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"*). Cố vấn dịch vụ phải đọc, gọi lại hỏi thêm rồi tự đoán nhóm lỗi để xếp đúng kỹ thuật viên/khoang sửa — chậm và hay xếp sai. |
| 2 | **Xanh SM** | Lặp lại | Tài xế gửi ảnh + mô tả các vụ va quệt nhỏ; nhân viên đội xe nhập tay thông tin (biển số, vị trí, bộ phận hư hỏng) vào hệ thống quản lý đội xe và hồ sơ bảo hiểm. |
| 3 | **Vinhomes** | Tốn thời gian | Ban quản lý tòa nhà đọc thủ công từng phản ánh trên App Vinhomes Resident (mất nước, hỏng đèn, ồn ào, rác...) rồi chuyển tay tới đúng tổ kỹ thuật/an ninh/vệ sinh. |
| 4 | **Vinmec** | Pain từ người khác | Tổng đài viên trả lời lặp đi lặp lại câu hỏi chuẩn bị trước xét nghiệm/nội soi (nhịn ăn bao lâu, có uống thuốc được không); bệnh nhân phàn nàn chờ máy lâu. |
| 5 | **Vinpearl** | Tốn thời gian | Nhân viên Sales khối Group/MICE đọc email yêu cầu đặt phòng theo đoàn (dài, lẫn Anh–Việt, kèm file đính kèm) và nhập tay số phòng, ngày, bữa ăn, hội trường vào bảng tính trước khi báo giá. |
| 6 | **Xanh SM** | Pain từ người khác | Khách để quên đồ trên xe gọi tổng đài; tổng đài viên phải tra lịch sử chuyến thủ công và gọi nhiều lần mới liên lạc được đúng tài xế. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3: **#1 (VinFast phân loại lỗi xe)**, **#5 (Vinpearl email đặt phòng đoàn)**, **#6 (Xanh SM đồ thất lạc)**.

Card #6 được chọn **có chủ đích** để kiểm tra phản biện "có thật sự cần AI không?".

## Card #1 — VinFast: Phân loại sơ bộ lỗi xe từ mô tả tiếng Việt

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Mô tả lỗi xe bằng tiếng Việt tự do của khách khiến│
│ cố vấn dịch vụ mất nhiều thời gian đoán nhóm lỗi, gọi lại   │
│ hỏi thêm, và thường xếp sai kỹ thuật viên/khoang sửa.       │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Cố vấn dịch vụ (Service Advisor) tại xưởng;    │
│ tổng đài viên hotline; khách hàng (phải chờ/quay lại xưởng) │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gọi hotline/chat app mô tả triệu chứng (3')      │
│   → 2. Tổng đài viên ghi chú tự do vào CRM (4')             │
│   → 3. Cố vấn đọc ghi chú, gọi lại khách hỏi thêm, tự đoán  │
│        nhóm lỗi + mức khẩn (8') 🔴                          │
│   → 4. Xếp lịch khoang + KTV đúng chuyên môn, đặt phụ tùng  │
│        (5')                                                 │
│   → 5. Xe vào xưởng, KTV chẩn đoán thực tế                  │
│                                                             │
│ Bước nào tốn nhất? Bước 3 (⏱ ~8 phút/phiếu)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: đọc mô tả →   │
│ đề xuất nhóm lỗi (danh mục cố định), mức khẩn, 3 câu hỏi    │
│ làm rõ — cố vấn duyệt. Rule cứng bắt từ khóa an toàn        │
│ (phanh, lái, khói, pin nóng) để escalate ngay.              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Bước 3 giảm từ ~8 phút → ≤ 3 phút/phiếu                   │
│ - ≥ 85% đề xuất nhóm lỗi được cố vấn chấp nhận không sửa    │
│ - 100% ca có triệu chứng an toàn được gắn cờ khẩn           │
│                                                             │
│ Quick Architecture: [x] Rule + [x] LLM (hybrid)             │
└─────────────────────────────────────────────────────────────┘
```

## Card #2 — Vinpearl: Trích xuất yêu cầu từ email đặt phòng theo đoàn

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Email yêu cầu đặt phòng đoàn từ công ty lữ hành   │
│ dài, không theo mẫu, lẫn Anh–Việt; nhân viên phải đọc và    │
│ nhập tay trước khi kiểm tra quỹ phòng và báo giá.           │
│ Công ty thành viên: [x] Khác: Vinpearl                      │
│                                                             │
│ Ai đang đau? Nhân viên Sales/Reservation khối Group & MICE; │
│ đối tác lữ hành (chờ báo giá lâu → chuyển sang đối thủ)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Nhận email yêu cầu (kèm Excel/PDF)                     │
│   → 2. Đọc & nhập tay ngày, số phòng theo loại, số khách,   │
│        bữa ăn, hội trường vào bảng tính (15') 🔴            │
│   → 3. Kiểm tra quỹ phòng trên PMS (5')                     │
│   → 4. Soạn email báo giá/xác nhận (10') 🔴                 │
│   → 5. Trưởng nhóm duyệt giá, gửi đối tác                   │
│                                                             │
│ Bước nào tốn nhất? Bước 2 + 4 (⏱ ~25 phút/email)            │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: trích xuất    │
│ thành JSON có cấu trúc; Bước 4: draft email từ JSON đã được │
│ nhân viên xác nhận. Giá luôn lấy từ hệ thống, không do AI.  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian từ nhận email → có báo giá nháp: ~35' → < 10'   │
│ - Độ chính xác trường ngày/số phòng ≥ 97% trên 100 email mẫu│
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — Xanh SM: Xử lý khách để quên đồ trên xe

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Khách quên đồ trên xe phải gọi tổng đài; tổng đài │
│ tra chuyến thủ công và gọi nhiều lần mới gặp được tài xế.   │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau? Khách hàng (lo lắng, chờ lâu); tổng đài viên   │
│ CSKH; tài xế (bị gọi khi đang chạy cuốc khác)               │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách gọi tổng đài, mô tả chuyến (giờ, điểm đón) (3')  │
│   → 2. Tra lịch sử chuyến theo SĐT để tìm tài xế (5') 🔴    │
│   → 3. Gọi tài xế xác nhận, thường không bắt máy (5') 🔴    │
│   → 4. Hẹn điểm/giờ trả đồ, ghi phiếu (4')                  │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ ~10 phút/lượt)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? KHÔNG CẦN AI.         │
│ Dữ liệu đã có cấu trúc (trip_id, SĐT, giờ, tài xế). Chỉ cần │
│ nút "Báo quên đồ" trong lịch sử chuyến trên app khách →     │
│ tự gắn trip_id → push thông báo cho đúng tài xế.            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian xác định tài xế: ~10' → < 1' (tự động)          │
│ - ≥ 80% đồ thất lạc được hẹn trả trong 24h                  │
│                                                             │
│ Quick Architecture: [x] Rule (No LLM)                       │
└─────────────────────────────────────────────────────────────┘
```

---

# 🧪 Stress-test các thẻ (vai CFO / Trưởng phòng Vận hành khó tính)

| Card | Phản biện | Kết luận sau phản biện |
|---|---|---|
| #1 VinFast | "Sao không dùng form chọn triệu chứng (dropdown) thay vì LLM?" — Khách thường không biết gọi tên bộ phận; form dài làm khách bỏ ngang, và kênh hotline vẫn là lời nói tự do. Tuy vậy, **phần phát hiện triệu chứng an toàn phải là rule cứng**, không giao cho LLM. | Giữ hybrid: Rule cho an toàn, LLM cho phần hiểu ngôn ngữ mơ hồ. |
| #2 Vinpearl | "Sai một con số ngày/số phòng là mất tiền thật." — Đúng. AI chỉ trích xuất, nhân viên xác nhận từng trường trước khi báo giá; giá lấy từ hệ thống. | Giữ LLM Feature, bắt buộc HITL ở bước xác nhận. |
| #3 Xanh SM | "Đây là bài toán sản phẩm/dữ liệu, không phải AI." — Đồng ý hoàn toàn. | **Rule-based tốt hơn**, không dùng AI. |

---

# 🗳️ Đề xuất bài toán cho Deep-Dive

Đề xuất chọn **Card #1 — VinFast phân loại sơ bộ lỗi xe**.

* **Chọn #1:** bottleneck nằm đúng ở phần xử lý ngôn ngữ tự nhiên mơ hồ (điểm mạnh của LLM), tác động cả hiệu suất xưởng lẫn an toàn khách hàng. Ranh giới vận hành rõ và kiểm soát được: AI chỉ *đề xuất*, cố vấn *quyết định*.
* **Không chọn #2:** phù hợp LLM nhưng tác động hẹp hơn (chỉ khối Group/MICE). Đây là ứng viên tốt cho vòng sau.
* **Không chọn #3:** không cần AI. Nên chuyển cho team sản phẩm Xanh SM làm tính năng rule-based.

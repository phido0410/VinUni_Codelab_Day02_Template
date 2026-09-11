# 🔍 Lab 02 — Deliverable 01: Problem Scan & Quick Cards (Green SM / Xanh SM)

**Đơn vị:** Vin Smart Future — Khối Công nghệ Vận hành  
**Công ty thành viên phụ trách:** Xanh SM (GSM — Green Smart Mobility)  
**Tác giả:** Kỹ sư AI Product Scoping  

---

# Phase 1 — SCAN: Bảng Quét Cơ Hội Tối Ưu Bằng AI (Xanh SM)

Sử dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác) để quét qua toàn bộ các mắt xích vận hành của **Xanh SM (GSM)** — từ điều vận cuốc xe, hỗ trợ tài xế thực địa, đến trải nghiệm của hành khách:

| # | Subsidiary | Lens | Mô tả ngắn bài toán & Điểm nghẽn vận hành |
|---|------------|------|-------------------------------------------|
| 1 | **Xanh SM** | Lặp lại (Repetitive) | **Tái phân bổ cuốc xe tự động khi khách đổi lộ trình giữa chừng:** Khách hàng thay đổi điểm đến hoặc thêm điểm dừng, hệ thống phải tính toán lại biểu phí và tái điều phối tài xế gần nhất mà không làm gián đoạn chuyến đi. |
| 2 | **Xanh SM** | Tốn thời gian (Time-consuming) | **Xử lý sự cố pin sạc khẩn cấp thực địa:** Tài xế báo cáo xe cạn pin (< 5%) hoặc trụ sạc dự kiến bị hỏng/quá tải. Điều phối viên mất 15-20 phút tra cứu bản đồ, kiểm tra trụ trống VinFast và soạn tin nhắn chỉ dẫn/gọi xe cứu hộ pin. |
| 3 | **Xanh SM** | AI có thể tốt hơn (AI-upgrade) | **Smart Dispatch Co-pilot chuẩn hóa điểm đón phức tạp:** Tin nhắn hoặc ghi chú của khách (ví dụ: *"đón tôi ở cổng phụ sau toà S2.05 gần quán nước mía"*) gây khó hiểu cho tài xế. AI phân tích văn bản/giọng nói để trích xuất mốc định vị chính xác. |
| 4 | **Xanh SM** | Pain từ người khác (Stakeholder Pain) | **Phân tích nguyên nhân gốc rễ hủy cuốc (Trip Cancellation & Drop-off Root Cause):** Khách và tài xế hủy chuyến sau khi ghép cuốc. Hệ thống hiện chỉ ghi nhận lý do chung chung; cần AI phân tích nhật ký cuộc gọi và chat để tìm điểm nghẽn (ví dụ: thời gian chờ quá lâu, tài xế không tìm được đường). |
| 5 | **Xanh SM** | Lặp lại & Tốn thời gian | **Trợ lý xử lý khiếu nại tài sản thất lạc (Lost & Found Matcher):** Hành khách quên đồ trên taxi điện (điện thoại, ví, hành lý). Nhân viên tổng đài mất 20-30 phút/vụ để gọi tài xế, tra cứu lịch trình cuốc và đối chiếu thông tin mô tả đồ vật. |
| 6 | **Xanh SM** | AI có thể tốt hơn (AI-upgrade) | **Dự báo nhu cầu và cân bằng pin đội xe (Fleet Battery Rebalancing):** Dự đoán trước 1-2 giờ các cụm xe có mức pin yếu trong khu vực trọng điểm và tự động đề xuất lộ trình về các trạm sạc VinFast vắng khách trước giờ cao điểm để tránh thiếu xe giờ vàng. |

---

# Phase 2 — QUICK-ASSESS: 3 Thẻ Bài Toán Tiềm Năng (Quick Problem Cards)

Từ danh sách trên, chọn ra **Top 3 bài toán** có tính cấp thiết và giá trị kinh doanh cao nhất của Xanh SM để hoàn thiện 3 Thẻ bài toán:

---

### 🃏 QUICK PROBLEM CARD #1: Xử lý sự cố pin sạc khẩn cấp thực địa cho tài xế Xanh SM

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán: Hỗ trợ khẩn cấp tài xế Xanh SM khi xe điện báo cạn pin (< 5%)    │
│ hoặc gặp sự cố tại trạm sạc, cần tìm trụ sạc trống gần nhất hoặc điều xe     │
│ cứu hộ pin di động ngay lập tức.                                            │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM (nguy cơ chết máy giữa đường, stress)   │
│ và Điều phối viên trung tâm (quá tải thao tác thủ công vào giờ cao điểm).    │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Nhận cuộc gọi/tin nhắn khẩn cấp từ tài xế                              │
│   ──> 2. Tra cứu tọa độ GPS của xe trên dashboard fleet management          │
│   ──> 3. Tra cứu hệ thống bản đồ trạm sạc VinFast tìm trụ trống phù hợp     │
│   ──> 4. Soạn thảo văn bản chỉ dẫn đường đi gửi qua app cho tài xế          │
│   ──> 5. Điều xe cứu hộ pin lưu động nếu pin < 5% hoặc trạm ở quá xa         │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & Bước 4 (⏱ 12-15 phút/lượt)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4                        │
│ (Auto-pull vị trí & trạng thái trạm sạc -> LLM draft tin nhắn hướng dẫn)    │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm thời gian xử lý từ 15-20 phút/lượt ──> dưới 3 phút/lượt.           │
│   - 100% trường hợp pin < 5% được cảnh báo và xử lý cứu hộ an toàn.         │
│   - Tỉ lệ gợi ý chính xác trạm sạc tương thích đạt >= 98%.                   │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule-based Router & HITL)      │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 🃏 QUICK PROBLEM CARD #2: Smart Dispatch Co-pilot chuẩn hóa điểm đón phức tạp

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán: Chuẩn hóa và làm rõ mô tả điểm đón khách tại các khu đô thị phức  │
│ tạp, ngõ hẻm Việt Nam từ tin nhắn ghi chú và voice note của hành khách.     │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Hành khách (chờ lâu, bực bội vì tài xế không thấy) và  │
│ Tài xế Xanh SM (loay hoay tìm đường trong ngõ, nguy cơ bị trừ sao/hủy cuốc).│
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Khách đặt xe, nhập địa chỉ chung và gõ thêm ghi chú/gọi tài xế         │
│   ──> 2. Tài xế vừa lái xe vừa đọc tin nhắn/nghe điện thoại của khách       │
│   ──> 3. Tài xế dừng lại hỏi thăm người dân xung quanh hoặc gọi lại xác nhận│
│   ──> 4. Tài xế tiếp cận điểm đón thực tế (thường lệch 100-300m so với map) │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 3 (⏱ 5-8 phút/cuốc)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & Bước 2                       │
│ (Trích xuất landmark, tên ngõ, vị trí mốc cụ thể và pin điểm đón chính xác)  │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm tỉ lệ cuộc gọi xác nhận giữa tài xế và khách từ 65% ──> dưới 20%.  │
│   - Rút ngắn thời gian tài xế tiếp cận đúng khách từ 7 phút ──> dưới 3 phút. │
│   - Giảm 30% tỉ lệ hủy chuyến do không tìm thấy điểm đón.                   │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (NER & Landmark Geocoding Assistant)    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 🃏 QUICK PROBLEM CARD #3: Trợ lý xử lý khiếu nại tài sản thất lạc (Lost & Found Matcher)

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Bài toán: Tự động tiếp nhận, trích xuất đặc điểm đồ thất lạc và so khớp     │
│ cuốc xe tương ứng để nhanh chóng liên hệ tài xế trao trả tài sản cho khách. │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Khách hàng bị mất đồ (lo lắng, sốt ruột) và Nhân viên   │
│ CSKH Xanh SM (mất hàng chục phút tra cứu log chuyến và đối soát biển số xe).│
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Khách hàng gửi ticket/gọi hotline trình báo mất đồ                     │
│   ──> 2. CSKH tra cứu lịch sử cuốc xe của khách theo số điện thoại          │
│   ──> 3. CSKH gọi điện cho tài xế chuyến đó để kiểm tra băng ghế sau/cốp    │
│   ──> 4. Soạn biên bản bàn giao và hẹn lịch trao trả                        │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 3 (⏱ 20-30 phút/vụ)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2 và 3                        │
│ (Phân loại đồ vật từ text/ảnh -> Tự động so khớp cuốc -> Gửi ping tài xế)   │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm thời gian kết nối tìm tài sản từ 45 phút ──> dưới 8 phút.           │
│   - Tỉ lệ tìm lại đồ vật thành công trong ngày tăng từ 72% ──> 92%.          │
│                                                                             │
│ Quick Architecture: [x] LLM Feature kết hợp Multimodal Vision               │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết Định Lựa Chọn Bài Toán Deep-Dive (Phase 3)

Nhóm quyết định chọn bài toán **Quick Problem Card #1: Xử lý sự cố pin sạc khẩn cấp thực địa cho tài xế Xanh SM** để tiến hành phân tích chuyên sâu (Deep-Dive) và xây dựng bản mẫu kỹ thuật (Technical Prompt Prototype).

### Lý do lựa chọn:
1. **Tính khẩn cấp và Tác động vận hành trực tiếp (Real-time Criticality):** Đối với xe taxi điện, cạn kiệt pin giữa đường không chỉ làm mất doanh thu chuyến xe mà còn gây rủi ro an toàn giao thông nghiêm trọng và chi phí cứu hộ kéo xe đắt đỏ.
2. **Ranh giới an toàn (Operational Boundary) rõ ràng và nghiêm ngặt:** Bài toán có các ngưỡng định lượng cụ thể (mức pin < 5%, khoảng cách trạm sạc > 5km, bắt buộc điều phối viên duyệt trước khi gửi tin), hoàn hảo để xây dựng và kiểm thử kỹ thuật Prompt Engineering với cơ chế Human-in-the-loop (HITL).
3. **Giá trị kinh tế lớn (High ROI):** Với quy mô hàng chục nghìn xe Xanh SM hoạt động hằng ngày, việc rút ngắn thời gian xử lý sự cố từ 15-20 phút xuống dưới 3 phút sẽ trực tiếp giải phóng hàng chục giờ công của điều phối viên mỗi ngày và tăng tỉ lệ khả dụng của đội xe.

### Lý do tạm hoãn hai bài toán còn lại:
* **Card #2 (Smart Dispatch Landmark Geocoding):** Đòi hỏi tích hợp sâu vào bản đồ nội bộ và hạ tầng GIS chuyên biệt, rủi ro ảo giác địa chỉ cần thời gian tinh chỉnh mô hình bản đồ trước khi triển khai thực địa.
* **Card #3 (Lost & Found Matcher):** Quy trình hậu mãi (back-office/after-sales), không mang tính thời gian thực và không gây nghẽn trực tiếp trên dòng xe đang lưu thông ngoài đường.


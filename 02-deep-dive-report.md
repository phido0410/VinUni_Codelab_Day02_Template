# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.



## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên vận hành tại từng ride (quan sát trực tiếp), quản lý khu vực (area supervisor) tiếp nhận báo cáo và ra quyết định điều phối, và quản lý vận hành công viên (Ops Manager) phê duyệt điều động nhân sự chéo khu vực khi vượt thẩm quyền. |
| **2. Current Workflow** | Nhân viên quan sát hàng đợi bằng mắt → báo cáo qua bộ đàm cho quản lý khu vực → quản lý đánh giá tình huống → nếu cần điều động chéo khu vực, xin phê duyệt cấp cao hơn → thực thi (mở thêm làn, điều nhân sự, thông báo loa). Không có công cụ số hóa nào hỗ trợ; toàn bộ dựa vào quan sát trực quan, bộ đàm và giấy/bảng ghi chú thủ công. |
| **3. Bottleneck** | Bước 3-4: chuỗi phê duyệt điều động nhân sự chéo khu vực (quản lý khu vực → cấp cao hơn) — không có dữ liệu định lượng để ra quyết định nhanh, và quy trình phê duyệt qua nhiều cấp gây trễ phản ứng. Đây cũng là bước phù hợp nhất để AI hỗ trợ (dự đoán điểm nóng + gợi ý điều phối tự động), dù không phải xử lý ngôn ngữ tự nhiên mà là computer vision / dữ liệu ticketing real-time. |
| **4. Business Impact** | Tổng thời gian xử lý một lượt quá tải ước tính 27-48 phút/lượt (từ phát hiện đến xử lý xong — số liệu minh họa, cần đối chiếu thực tế). Hệ quả: giảm throughput khách/giờ tại điểm nóng ước tính 15-20% theo benchmark ngành, kéo theo giảm doanh thu upsell F&B/retail và giảm NPS 10-15 điểm do trải nghiệm chờ đợi kém. |
| **5. Success Metric** | Ví dụ: "Giảm thời gian phát hiện + phản ứng quá tải từ ~30 phút xuống dưới 5 phút", hoặc "Tăng throughput khách/giờ tại các ride hot thêm 10-15%". Cần xác nhận baseline thực tế tại Vinpearl trước khi chốt ngưỡng chính thức. |
| **6. Operational Boundary** | AI được phép: đếm/dự đoán mật độ khách qua camera hoặc dữ liệu ticketing, gợi ý điều phối nhân sự/mở làn cho quản lý khu vực xem xét, cảnh báo sớm nguy cơ quá tải. AI tuyệt đối không được: tự động ra lệnh điều chuyển nhân sự mà không qua xác nhận của quản lý (an toàn vận hành, trách nhiệm pháp lý), không được xử lý dữ liệu khuôn mặt để định danh cá nhân (rủi ro privacy). Điểm cần duyệt: mọi quyết định điều chuyển nhân sự chéo khu vực và mọi thay đổi ảnh hưởng đến an toàn vận hành ride vẫn cần quản lý khu vực xác nhận trước khi thực thi. |


## 3.3. Future-State Flow & AI Fit
Bài toán: Khách xếp hàng quá lâu ở trò chơi hot, khu vực khác vắng — thiếu điều phối real-time

**AI-Fit Matrix:**
[X] Rule / State-Machine   [ ] LLM Feature   [X] Agentic Loop (lớp tổng hợp gợi ý)

┌────────────────────────────────────────────────────────────────────────┐
│  BƯỚC 1                     BƯỚC 2                    BƯỚC 3            │
│  🔵 AI Step:            ──> 🔵 AI Step:            ──> 🟢 Human Step:    │
│  State-machine đọc số        Agent tổng hợp mức độ      Quản lý khu vực  │
│  liệu quét vé/camera         quá tải theo từng ride,    xem gợi ý, xác   │
│  real-time, tính tốc độ      xếp hạng ride nào cần      nhận hoặc điều   │
│  tích lũy khách/15 phút      ưu tiên điều phối trước    chỉnh quyết định │
│  theo từng ride                                         (HITL)          │
│  ⏱ liên tục, ~real-time      ⏱ <30 giây                 ⏱ 1-2 phút       │
│                                                                          │
│         │                                                               │
│         ↩️ Fallback: nếu dữ liệu quét vé/camera bị gián đoạn hoặc        │
│            confidence thấp (VD: che khuất, lỗi cảm biến) → quay về      │
│            quy trình thủ công hiện tại (nhân viên báo cáo qua bộ đàm),  │
│            đồng thời cảnh báo rõ cho quản lý biết hệ thống đang fallback │
│                                                                          │
│                                          ▼                              │
│                                    BƯỚC 4                               │
│                                    🟢 Human Step:                       │
│                                    Quản lý phê duyệt điều động           │
│                                    nhân sự chéo khu vực (nếu cần)        │
│                                    ⏱ <5 phút (rút ngắn từ 10-15 phút     │
│                                    nhờ có dữ liệu định lượng hỗ trợ)     │
│                                                                          │
│                                          ▼                              │
│                                    BƯỚC 5                               │
│                                    🟢 Human Step:                       │
│                                    Thực thi điều chỉnh (mở làn,          │
│                                    điều nhân sự, thông báo loa)          │
│                                    ⏱ 5-10 phút (không đổi — vẫn cần      │
│                                    thao tác vật lý con người)            │
│                                                                          │
│                                          ▼                              │
│                                    BƯỚC 6 (MỚI)                         │
│                                    🔵 AI Step:                          │
│                                    Tự động ghi log (giờ, ride, mức độ    │
│                                    quá tải, hành động đã thực hiện) để   │
│                                    huấn luyện lại ngưỡng cảnh báo        │
│                                    ⏱ tự động, không tốn thời gian người  │

  Tổng cộng (dự kiến) = ~10-18 phút/lượt (giảm từ 27-48 phút hiện tại)
└────────────────────────────────────────────────────────────────────────┘

Nguyên tắc thiết kế:
- AI KHÔNG tự động ra lệnh điều chuyển nhân sự — chỉ gợi ý, con người luôn
  là điểm quyết định cuối (Bước 3, 4).
- Fallback về quy trình thủ công là bắt buộc, không phải tùy chọn — đảm bảo
  vận hành không bị gián đoạn nếu hệ thống AI lỗi/mất kết nối.
- Bước 6 (ghi log tự động) là bước AI-native mới, không tồn tại ở current-state,
  tạo vòng phản hồi để cải thiện độ chính xác ngưỡng cảnh báo theo thời gian.
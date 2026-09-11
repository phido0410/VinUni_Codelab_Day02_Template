# 🏗️ Lab 02 — Deliverable 02: Deep-Dive Report & Evaluation (Green SM / Xanh SM)

**Đơn vị:** Vin Smart Future — Khối Công nghệ Vận hành  
**Dự án:** Xanh SM Dispatch Co-pilot — Hệ thống Trợ lý Điều phối Sự cố Pin Thực địa  
**Công ty thành viên:** Xanh SM (GSM — Green Smart Mobility)  
**Tác giả:** Kỹ sư AI Product Scoping  

---

# 1. Bối cảnh & Tầm nhìn Dự án

**Xanh SM (GSM)** hiện là đơn vị tiên phong tại Việt Nam và Đông Nam Á vận hành 100% đội xe thuần điện (VinFast VF e34, VF 5 Plus, VF 8, Feliz S...). Với quy mô hàng chục nghìn lượt xe lăn bánh mỗi ngày, việc duy trì tính khả dụng của pin và giảm thiểu thời gian "chết" do cạn năng lượng giữa đường là yếu tố sống còn đối với hiệu suất vận hành và trải nghiệm khách hàng.

Báo cáo này phân tích chuyên sâu quy trình xử lý sự cố pin và đề xuất giải pháp tích hợp AI — **Xanh SM Dispatch Co-pilot** — nhằm chuyển đổi quy trình thủ công kéo dài 15-20 phút thành quy trình bán tự động có con người giám sát (Human-in-the-loop) dưới 3 phút.

---

# 🏗️ Phase 3 — DEEP-DIVE: Phân Tích Quy Trình & Ranh Giới Vận Hành

## 3.1. Current-State Workflow Mapping (Quy trình Vận hành Hiện tại)

Khi xe taxi điện Xanh SM gặp sự cố pin thực địa (pin dưới 5% hoặc trạm sạc dự kiến gặp sự cố không thể nạp), quy trình điều phối viên xử lý hiện tại diễn ra như sau:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Tiếp nhận    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn thảo    │
│ cuộc gọi/app │ ──> │ vị GPS của xe│ ──> │ sạc VinFast  │ ──> │ tin nhắn     │
│ báo sự cố    │     │ trên Fleet UI│     │ còn trụ trống│     │ chỉ dẫn đường│
│              │ 🔄  │              │ 🔄  │              │     │              │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 6 phút 🔴  │     │ ⏱ 6 phút 🔴  │
│ In: Phone/App│     │ In: Biển số  │     │ In: Vị trí, xe│    │ In: Trạm, km │
│ Out: Log vé  │     │ Out: Tọa độ  │     │ Out: Địa chỉ │     │ Out: SMS/Chat│
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼ 🔄
                                                                ┌──────────────┐
                                                                │ Bước 5       │
                                                                │ Liên hệ điều │
                                                                │ Xe cứu hộ pin│
                                                                │ (nếu pin kiệt)│
                                                                │              │
                                                                │ Ai: Dispatch │
                                                                │ ⏱ 2 phút     │
                                                                │ In: Lệnh điều│
                                                                │ Out: Phái xe │
                                                                └──────────────┘

🔴 Bottleneck: Bước 3 và Bước 4 tiêu tốn 12 phút do phải đối soát đa hệ thống thủ công.
🔄 Handoff: Điểm chuyển giao thông tin giữa điện thoại tài xế -> Fleet dashboard -> Bản đồ trạm sạc -> Kênh SMS.
⏱ Tổng thời gian xử lý thủ công: 15-20 phút / sự cố.
```

### Chi tiết các bước và điểm nghẽn:
1. **Bước 1 (Tiếp nhận):** Tài xế gọi hotline hoặc bấm nút SOS trên app tài xế báo cạn pin hoặc không cắm được sạc. Điều phối viên lắng nghe, ghi chép dòng xe và tình trạng. (2 phút)
2. **Bước 2 (Tra cứu GPS):** Điều phối viên nhập biển số xe vào màn hình Fleet Management để lấy tọa độ thời gian thực. (2 phút)
3. **Bước 3 (Tra cứu trạm sạc trống — 🔴 Bottleneck):** Mở bản đồ trạm sạc VinFast, đo cự ly bán kính, lọc trụ sạc tương thích (ví dụ: VF5 sạc trụ 30kW-60kW, VF8 sạc trụ siêu nhanh 150kW-250kW), kiểm tra trạng thái trụ còn trống không bị chiếm chỗ. (6 phút)
4. **Bước 4 (Soạn tin nhắn hướng dẫn — 🔴 Bottleneck):** Gõ tay từng câu chữ mô tả đường đi, tên tòa nhà/trung tâm thương mại có trạm sạc, ghi chú cảnh báo tắt điều hòa tiết kiệm điện để gửi cho tài xế. (6 phút)
5. **Bước 5 (Điều xe cứu hộ pin):** Nếu pin dưới 5% hoặc trạm quá xa, điều phối viên phải gọi sang bộ phận xe cứu hộ pin lưu động (Mobile Charger Truck) để xếp lịch cứu hộ kéo xe hoặc nạp khẩn cấp. (2 phút)

---

## 3.2. Problem Statement (6-Field Framework) — Vin Smart Future Standard

| Trường thông tin | Nội dung chi tiết bài toán Xanh SM |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM (Hà Nội, TP.HCM, Đà Nẵng). |
| **2. Current Workflow** | Điều phối viên tiếp nhận cuộc gọi, tự tra cứu GPS xe, mở tab bản đồ VinFast kiểm tra trụ trống phù hợp theo model xe, gõ tin nhắn hướng dẫn đường đi bằng tay, và gọi xe cứu hộ nếu xe cạn kiệt pin. Quy trình 5 bước thủ công hoàn toàn qua nhiều màn hình tách rời. |
| **3. Bottleneck** | **Bước 3 & Bước 4 (ngốn 12 phút):** Tra cứu thủ công trụ sạc trống khả dụng theo cổng sạc của từng dòng xe điện và gõ tin nhắn hướng dẫn chi tiết bằng tiếng Việt trong áp lực tài xế đang đợi sốt ruột. |
| **4. Business Impact** | Mỗi ngày có trung bình **~80 - 100 sự cố pin thực địa** tại các thành phố lớn. Tiêu tốn hơn **25 giờ công làm việc/ngày** của đội ngũ điều phối viên. Tăng thời gian chờ đợi của tài xế lên 15-20 phút, gây ách tắc giao thông, làm rò rỉ khoảng **15-20% doanh thu ca chạy** do gián đoạn phục vụ khách hàng. |
| **5. Success Metric** | **1. Hiệu suất (Efficiency):** Giảm tổng thời gian xử lý sự cố từ **18 phút xuống dưới 3 phút/lượt** (giảm > 80%).<br>**2. Độ chính xác (Quality):** Tỉ lệ chỉ dẫn đúng trạm sạc tương thích và còn trụ trống đạt **>= 98%**.<br>**3. An toàn (Safety):** **100%** trường hợp xe dưới 5% pin được cảnh báo nguy cấp và kích hoạt điều xe cứu hộ pin kịp thời, không để xe chết máy giữa đường. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Tự động gọi API lấy tọa độ GPS, trích xuất dòng xe, truy vấn danh sách trụ sạc trống từ VinFast API, phân loại mức độ nguy cấp, và soạn thảo bản nháp (draft) tin nhắn.<br>**AI TUYỆT ĐỐI CẤM:**<br>1. *CẤM tự ý gửi tin:* Không được tự động gửi tin nhắn cho tài xế khi chưa có Điều phối viên bấm duyệt (bắt buộc nhãn `[DRAFT_ONLY]`).<br>2. *CẤM điều xe đi xa khi pin nguy cấp:* Nếu pin < 5%, tuyệt đối không gợi ý trạm sạc cách xa quá 5km (bắt buộc chuyển sang action `dispatch_mobile_charger`).<br>3. *CẤM bịa dữ liệu:* Không được tự suy diễn địa chỉ hay số trụ nếu hệ thống chưa trả về dữ liệu. |

---

## 3.3. Future-State Flow & Đánh Giá Mức Độ Phù Hợp AI (AI-Fit Matrix)

### 📊 Phân tích AI-Fit Matrix: Tại sao chọn LLM Feature có HITL?

| Tiêu chí so sánh | Rule-based / State-Machine | **LLM Feature (LỰA CHỌN)** | Autonomous Multi-Agent |
|---|---|---|---|
| **Khả năng hiểu ngữ cảnh sự cố** | Kém: Khó xử lý mô tả ngôn ngữ tự nhiên từ cuộc gọi/chat của tài xế. | **Xuất sắc:** Trích xuất nhanh dòng xe, % pin, tình trạng khẩn cấp từ tiếng Việt phong phú. | Dư thừa: Không cần hội thoại đa tác nhân phức tạp. |
| **Tốc độ xử lý** | Rất nhanh (< 100ms) nhưng cứng nhắc. | **Nhanh & Linh hoạt (1-2s):** Tạo tin nhắn chỉ dẫn thân thiện, cá nhân hóa theo tình huống. | Chậm (5-15s do nhiều vòng suy nghĩ loop), dễ timeout. |
| **Rủi ro vận hành** | Thấp, nhưng tỉ lệ false-alarm cao khi dữ liệu không chuẩn. | **Kiểm soát tuyệt đối qua HITL:** Điều phối viên kiểm tra và click duyệt trước khi gửi. | Cực cao: Nếu agent tự quyết định điều xe sai sẽ gây thiệt hại chi phí lớn. |
| **Kết luận lựa chọn** | Dùng làm Router phân luồng ban đầu. | **Trọng tâm của giải pháp: Trợ lý Co-pilot hỗ trợ soạn thảo và tổng hợp.** | Không phù hợp ở giai đoạn hiện tại. |

---

### 🔄 Sơ đồ Quy trình Tương lai (Future-State Flow):

```text
┌─────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
│ Bước 1          │     │ Bước 2               │     │ Bước 3               │     │ Bước 4               │
│ Tiếp nhận tín   │     │ 🔵 AI Engine         │     │ 🔵 LLM Co-Pilot      │     │ 🟢 Human-in-the-loop │
│ hiệu sự cố từ   │ ──> │ Auto-pull GPS &      │ ──> │ Phân loại rủi ro &   │ ──> │ Điều phối viên xem,  │
│ tài xế (SOS/Call│     │ Trạm VinFast trống   │     │ Soạn Draft [DRAFT_ONLY│    │ tinh chỉnh & Duyệt   │
└─────────────────┘     └──────────────────────┘     └──────────────────────┘     └──────────────────────┘
                                                                │                            │
                                                      Pin < 5%  │                  Bấm Duyệt │
                                                      & cự ly xa│                            ▼
                                                                ▼                  ┌──────────────────────┐
                                                       Kích hoạt lệnh:             │ Hệ thống gửi tin     │
                                                       "dispatch_mobile_charger"   │ chính thức cho tài xế│
                                                                │                  │ & điều xe cứu hộ     │
                                                                ▼                  └──────────────────────┘
                                                    ┌──────────────────────┐
                                                    │ ↩️ Fallback Kế hoạch:│
                                                    │ Nếu LLM phản hồi lỗi,│
                                                    │ timeout (>3s) hoặc   │
                                                    │ không tự tin, trả về │
                                                    │ giao diện tra cứu cũ.│
                                                    └──────────────────────┘
```

* **🔵 AI Step:** 
  - Bước 2: Tự động trích xuất thông tin thực địa qua API, lọc danh sách trạm sạc khả dụng.
  - Bước 3: Phân tích mức pin. Nếu pin < 5% và trạm > 5km -> chuyển chế độ cứu hộ lưu động (`dispatch_mobile_charger`). Nếu an toàn -> tạo bản nháp tin nhắn chỉ dẫn với tag bắt buộc `[DRAFT_ONLY]`.
* **🟢 Human Step (HITL):** Điều phối viên chỉ mất 15-30 giây đọc lướt bản nháp được AI chuẩn bị sẵn, sửa nhanh nếu cần và ấn nút **[GỬI TÀI XẾ]** hoặc **[XÁC NHẬN ĐIỀU XE CỨU HỘ]**.
* **↩️ Fallback Mechanism:** Nếu dịch vụ AI gặp sự cố kết nối, timeout quá 3 giây, hoặc dữ liệu đầu vào thiếu trường quan trọng (`need_more_info`), giao diện sẽ tự động chuyển sang chế độ thao tác tay truyền thống mà không làm gián đoạn ca trực.

---

# 💻 Phase 4 — Kiểm Thử Bản Mẫu Kỹ Thuật (Prompt Prototype)

Hệ thống đã được lập trình nguyên mẫu bằng Python tại file `starter-code/prompt_prototype.py` sử dụng **Gemini 2.5 Flash SDK** với các ranh giới an toàn nghiêm ngặt:
1. **Bảo vệ tính pháp lý & an toàn:** Buộc dòng đầu tiên của câu trả lời luôn chứa tag `[DRAFT_ONLY]`.
2. **Bảo vệ phương tiện:** Pin dưới 5% từ chối chỉ dẫn trạm sạc xa > 5km, bắt buộc kích hoạt `dispatch_mobile_charger`.
3. **Phòng chống tấn công Prompt Injection:** Chống lại các nỗ lực giả mạo quyền quản lý `[SYSTEM OVERRIDE]` hay yêu cầu bỏ định dạng JSON.

Chi tiết kiểm thử và kết quả assertions được ghi nhận tại Phase 4 trong code và phản ánh trong file `03-ai-log.md`.

---

# 🏁 Phase 5 — EVALUATE: Đánh Giá Sẵn Sàng & Quyết Định Đầu Tư

### 1. Bảng Đánh Giá Mức Độ Sẵn Sàng (AI Readiness Checklist):

| Câu hỏi thẩm định | Đánh giá | Bằng chứng thực tế tại Xanh SM / Vingroup |
|---|:---:|---|
| **1. Có sẵn dữ liệu logs sạch để kiểm thử không?** | **CÓ (READY)** | Hệ thống Telematics của VinFast và cơ sở dữ liệu trạm sạc VinFast ghi nhận toạ độ GPS, % pin (SOC), trạng thái trụ sạc theo thời gian thực (update mỗi 10-30s). |
| **2. Rủi ro khi AI sai sót có nằm trong tầm kiểm soát không?** | **CÓ (CONTROLLED)** | Rất an toàn nhờ cơ chế **HITL 100%** (tag `[DRAFT_ONLY]`, nhân viên luôn là người bấm nút gửi) kết hợp **Fallback tự động** quay về tra cứu truyền thống khi có nghi ngờ. |
| **3. Stakeholders (Tài xế, Điều phối viên) sẵn sàng đổi mới?** | **CÓ (HIGH DEMAND)** | Đội ngũ điều phối viên đang bị quá tải nghiêm trọng vào giờ cao điểm, rất mong muốn có công cụ tự động gom thông tin và draft sẵn câu trả lời. |

---

### 2. Quyết Định Đầu Tư Của Ban Giám Đốc Vin Smart Future:

# ✅ QUYẾT ĐỊNH: [ GO ] — BẮT ĐẦU XÂY DỰNG PROTOTYPE HẸP (PILOT)

### 3. Luận giải chi tiết (Justification):
1. **Giá trị Kinh Tế & ROI rõ ràng:**
   - Tại Hà Nội và TP.HCM, mỗi ngày có ~80-100 sự cố pin. Tiết kiệm 15 phút/sự cố = giải phóng **20-25 giờ công điều vận/ngày**, tương đương cắt giảm áp lực tuyển dụng thêm 3-4 nhân sự trực ca.
   - Giúp xe quay lại chu kỳ đón khách sớm hơn 15 phút, cứu vãn hàng trăm triệu đồng doanh thu cuốc xe mỗi tháng không bị hủy.
2. **Độ phức tạp kỹ thuật vừa phải, tính khả thi cao:**
   - Không cần huấn luyện mô hình nền tảng mới (no fine-tuning required); chỉ cần áp dụng **Prompt Engineering nghiêm ngặt + Function Calling / Tool Use** để truy xuất API định vị VinFast.
   - Kiến trúc **LLM Feature + HITL** loại trừ hoàn toàn nguy cơ AI tự hành gửi thông tin sai cho tài xế.
3. **Lộ trình triển khai khuyến nghị:**
   - **Giai đoạn 1 (2 tuần):** Pilot nội bộ trên 1 ca trực ban đêm của Trung tâm Điều vận Xanh SM Hà Nội (quy mô 5 điều phối viên).
   - **Giai đoạn 2 (4 tuần):** Mở rộng cho toàn bộ đội ngũ điều vận Xanh SM toàn quốc sau khi đạt độ chính xác khuyến nghị >= 98%.


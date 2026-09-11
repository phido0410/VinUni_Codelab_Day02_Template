# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**.

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:

- 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
- 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
- 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
- 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
- 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate                         | Điểm | Deliverable       | Tiêu chí chấm                                                                |
| ---------------------------- | ---: | ----------------- | ---------------------------------------------------------------------------- |
| **G1. Workflow Mapping**     |   20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck     |
| **G2. Problem Statement**    |   20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** |   10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback  |
| **G4. Decision Quality**     |   10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng                |

### 👤 Điểm cá nhân (40 điểm)

| Gate                        | Điểm | Deliverable  | Tiêu chí chấm                                                                     |
| --------------------------- | ---: | ------------ | --------------------------------------------------------------------------------- |
| **I1. Scan & Cards**        |   15 | Quick Cards  | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng          |
| **I2. Prototyping**         |   10 | 02-lab/      | Chạy thử nghiệm programmatic prompt prototype thành công                          |
| **I3. AI Log & Reflection** |   15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

_Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI._
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:

1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> _"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."_

### 📝 List bài toán của tôi:

| #   | Subsidiary  | Lens               | Mô tả ngắn bài toán                                                                                                                                                                                                                                                                                                 |
| --- | ----------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **VinFast** | AI có thể tốt hơn  | **Chuẩn hóa & Bóc tách mô tả lỗi xe bằng tiếng Việt đời thường của khách tại Xưởng dịch vụ 3S:** Khách mang xe vào xưởng tả lỗi cảm tính (_"đi qua gờ giảm tốc 40km/h dưới gầm phụ kêu lục cục, trời mưa thì êm"_), Cố vấn dịch vụ (SA) ghi vắn tắt khiến thợ xưởng mất cả tiếng chạy thử xe ngoài đường để mò lỗi. |
| 2   | **VinFast** | Tốn thời gian      | **Phân tích log bắt tay (Handshake) & lỗi ngắt sạc bất thường tại trạm sạc V-GREEN:** Khi trụ sạc ngắt sạc giữa chừng hoặc xe không nhận dòng, nhân viên kỹ thuật phải đọc log OCPP / CAN bus thủ công hàng nghìn dòng để xác định do trụ, do cáp sạc hay do BMS của xe.                                            |
| 3   | **VinFast** | Lặp lại            | **Tự động đối soát và phân bổ hóa đơn tiền điện trạm sạc đối tác:** Hằng tháng nhân viên kế toán vận hành phải so khớp hàng chục nghìn phiên sạc thực tế tại các điểm liên kết (chung cư, trạm dừng nghỉ) với hóa đơn tiền điện từ EVN gửi về.                                                                      |
| 4   | **VinFast** | Pain từ người khác | **Dự đoán lệch điện áp cell pin (Cell Imbalance) từ Telemetry xe:** Phân tích dữ liệu BMS định kỳ đẩy về cloud để cảnh báo chủ xe VF5/VF8 mang pin đi cân bằng trước khi xe bị sập nguồn đột ngột giữa đường khiến khách bức xúc.                                                                                   |
| 5   | **VinFast** | Tốn thời gian      | **Tổng hợp lỗi lặp lại từ phiếu sửa chữa (RO) phản hồi về R&D Cát Hải:** Đọc hàng chục nghìn phiếu tiếp nhận bảo dưỡng toàn quốc để gom nhóm các lỗi phổ biến (tiếng ồn nội thất, phần mềm màn hình treo) gửi kỹ sư cải tiến sản phẩm.                                                                              |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (CHỌN LÀM TRỌNG TÂM)                 │
│                                                             │
│ Bài toán: Bóc tách mô tả hiện tượng lỗi bằng tiếng Việt đời │
│ thường của khách hàng thành phiếu chẩn đoán kỹ thuật cho    │
│ thợ máy tại Xưởng dịch vụ VinFast 3S.                       │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ SA (quá tải tiếp nhận)  │
│ và Kỹ thuật viên xưởng (mất công chạy thử mò lỗi mù quáng). │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Khách kể bệnh ──> 2. SA ghi vắn tắt lên lệnh sửa chữa  │
│   ──> 3. Thợ xưởng nhận xe ──> 4. Thợ lái thử xe tìm tiếng  │
│   kêu ngoài đường ──> 5. Thợ gọi lại SA hỏi thêm chi tiết.  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 50 phút/xe)   │
│ AI nhảy vào ở bước nào? Bước 2: Bóc tách thuộc tính lỗi     │
│ (vị trí, âm thanh, vận tốc, thời tiết) & gợi ý điểm kiểm tra│
│ tĩnh trên cầu nâng cho thợ, dẹp bỏ việc chạy thử vô bổ.     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian tiếp nhận & ra phiếu từ 15 min ──> under 4m│
│ - Giảm tỉ lệ phải vác xe ra đường chạy thử từ 35% ──> < 10% │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Trích xuất có cấu trúc)│
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phân tích log sự cố ngắt sạc bất thường   │
│ tại trạm sạc công cộng V-GREEN để hỗ trợ hoàn tiền/giải tỏa.│
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng (bực mình vì xe sạc 2% đã   │
│ ngắt) và Nhân viên trực tổng đài hỗ trợ trạm sạc.           │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Khách gọi tổng đài ──> 2. Nhân viên mở dashboard trụ   │
│   ──> 3. Tải file log OCPP ──> 4. Đọc thủ công mã lỗi điện  │
│   ──> 5. Bấm lệnh mở khóa súng hoặc tạo vé hoàn tiền cọc.   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 12 phút/ca)   │
│ AI nhảy vào ở bước nào? Bước 4: Tự động parse file log và   │
│ phân loại nguyên nhân lỗi (do súng sạc, nguồn điện hay xe). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Rút ngắn thời gian xử lý khiếu nại trạm sạc từ 15m ──> 2m │
│                                                             │
│ Quick Architecture: [x] Rule + LLM                          │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Cảnh báo sớm lệch điện áp cell pin (Cell Voltage  │
│ Imbalance) từ dữ liệu Telemetry xe điện VinFast.            │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng (nguy cơ nằm đường) và      │
│ Kỹ sư vận hành hệ sinh thái pin VinFast.                    │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Telemetry bắn dữ liệu về Data Lake ──> 2. Kỹ sư chạy  │
│   job SQL lọc cell lệch ngưỡng ──> 3. Xuất danh sách Excel   │
│   ──> 4. Gửi email thủ công cho các xưởng dịch vụ gọi khách │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 3-4 ngày/đợt) │
│ AI nhảy vào ở bước nào? Bước 2 & 3: Tự động phát hiện bất   │
│ thường theo chuỗi thời gian và draft lịch hẹn cho xưởng 3S. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Phát hiện nguy cơ trước 72 giờ, giảm 80% ca xe nằm đường. │
│                                                             │
│ Quick Architecture: [x] Rule-based / ML (Không cần LLM)     │
└─────────────────────────────────────────────────────────────┘
```

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping

Quy trình tiếp nhận và kiểm tra lỗi âm thanh/lỗi vận hành tại Xưởng dịch vụ VinFast 3S hiện nay:

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Bước 1          │       │ Bước 2          │       │ Bước 3          │
│ Khách lái xe vào│       │ Cố vấn SA ghi   │       │ Handoff xe và   │
│ xưởng & mô tả   │ ───>  │ tóm tắt vào     │ ───>  │ phiếu RO cho    │
│ bệnh bằng lời kể│       │ phần mềm DMS    │       │ thợ xưởng       │
│ Ai: Khách + SA  │       │ Ai: SA          │       │ Ai: SA -> Thợ   │
│ ⏱ 8 phút        │       │ ⏱ 7 phút 🔴     │       │ ⏱ 5 phút 🔄     │
│ In: Lời nói thô │       │ Out: Phiếu RO   │       │ In: Giấy in RO  │
└─────────────────┘       └─────────────────┘       └─────────────────┘
                                                              │
                                                              ▼
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Bước 6          │       │ Bước 5          │       │ Bước 4          │
│ Đưa xe lên cầu  │       │ Thợ gọi lại SA  │       │ Thợ lấy xe ra   │
│ 2 trụ tháo gỡ   │ <───  │ yêu cầu SA gọi  │ <───  │ đường chạy thử  │
│ kiểm tra chi tiết│      │ khách làm rõ    │       │ mò mẫm nghe lỗi │
│ Ai: Kỹ thuật viên│      │ Ai: Thợ -> SA   │       │ Ai: Kỹ thuật viên│
│ ⏱ 30 phút       │       │ ⏱ 15 phút 🔄    │       │ ⏱ 45 phút 🔴    │
└─────────────────┘       └─────────────────┘       └─────────────────┘

🔴 Bottlenecks:
- Bước 2: SA vội tiếp nhận khách, ghi chú chung chung ("Khách báo gầm kêu"), làm mất sạch chi tiết bối cảnh.
- Bước 4: Thợ xưởng mù mờ thông tin, phải lái xe ra đường chạy thử 45 phút mò mẫm, dễ dẫn đến kết luận "Không thấy lỗi" (No Fault Found - NFF) khi đường không giống điều kiện khách đi hằng ngày.

🔄 Handoffs:
- Giữa Cố vấn dịch vụ (SA) và Kỹ thuật viên xưởng: Độ lệch pha ngôn ngữ giữa "người bán dịch vụ" và "người vặn ốc".
- Giữa Thợ xưởng ngược lại SA: Khi không nghe thấy tiếng kêu, thợ phải vòng về hỏi lại khách, gây trễ nải quy trình.

⏱ Tổng thời gian xử lý thủ công ban đầu: ~110 phút/lượt xe.
```

---

## 3.2. Problem Statement (6-field) & Metrics — Vin Smart Future Standard

| Field                       | Nội dung chi tiết                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Cố vấn dịch vụ (Service Advisor - SA) tại quầy tiếp nhận và Kỹ thuật viên (Technician) tại Xưởng dịch vụ VinFast 3S.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| **2. Current Workflow**     | Khách hàng đánh xe (VF 5, VF 8, VF 9, xe máy điện) vào xưởng và mô tả lỗi bằng ngôn ngữ đời thường mộc mạc (_"đi qua gờ giảm tốc tầm 40km/h là dưới gầm ghế phụ kêu lọc cọc, trời mưa thì êm"_). SA ghi vắn tắt vào phần mềm nội bộ DMS (_"xe kêu gầm"_). Kỹ thuật viên cầm phiếu không hiểu ngữ cảnh, phải mang xe ra đường chạy thử 30-45 phút để "săn" tiếng ồn hoặc cắm máy chẩn đoán quét mã DTC. Nếu không thấy mã lỗi lưu, xe bị xếp vào diện "No Fault Found" (NFF) và trả về cho khách, khiến khách mang xe về đi vẫn bị và bức xúc quay lại xưởng khiếu nại.                                                     |
| **3. Bottleneck**           | **Bước 2 & Bước 4:** Bước 2 mất 7 phút ghi chép thủ công nhưng chất lượng thông tin cực thấp do SA không có thời gian hỏi sâu. Bước 4 mất 45 phút công thợ chạy thử ngoài đường, vừa tốn thời gian, hao pin xe, vừa dễ gây va chạm giao thông ngoài xưởng.                                                                                                                                                                                                                                                                                                                                                                 |
| **4. Business Impact**      | Mỗi xưởng dịch vụ VinFast 3S tiếp nhận 40-50 lượt xe/ngày. Có khoảng 35% ca liên quan đến lỗi âm thanh khung gầm hoặc lỗi chập chờn. Hiện tượng NFF và chạy thử kéo dài làm lãng phí ~25 giờ công thợ/ngày tại mỗi xưởng, chiếm dụng cầu nâng, đẩy thời gian chờ của khách lên cao và làm sụt giảm chỉ số hài lòng dịch vụ (CSI - Customer Satisfaction Index).                                                                                                                                                                                                                                                            |
| **5. Success Metric**       | 1. Giảm thời gian SA lập phiếu tiếp nhận kỹ thuật từ 15 phút xuống dưới 4 phút.<br>2. Giảm tỉ lệ xe phải chạy thử ngoài đường để mò tiếng ồn từ 35% xuống dưới 10% (chuyển sang kiểm tra tĩnh trực tiếp trên cầu nâng).<br>3. Độ chính xác trích xuất đúng cụm linh kiện nghi ngờ (Subsystem Level) đạt $\ge 90\%$.                                                                                                                                                                                                                                                                                                        |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Trích xuất các trường thông tin chuẩn (vị trí âm thanh, vận tốc xuất hiện, điều kiện mặt đường, điều kiện loại trừ); đối chiếu với cẩm nang kỹ thuật TSB (Technical Service Bulletins) của VinFast để đưa ra danh sách cụm linh kiện nghi ngờ và hướng dẫn kiểm tra tĩnh trên cầu nâng; soạn bản nháp phiếu lệnh sửa chữa (Draft RO).<br>**AI TUYỆT ĐỐI CẤM:** Không được đưa ra kết luận chẩn đoán cuối cùng ("Khẳng định hỏng rô-tuyn cân bằng"); không được tự ý tạo lệnh xuất kho phụ tùng bảo hành thay thế; không được tự động gửi báo giá cho khách hàng trước khi Kỹ thuật viên trưởng ký duyệt. |

---

## 3.3. Future-State Flow & AI Fit

- **Đánh giá mức độ phù hợp AI (AI Fit):**
  - Không dùng **Rule-based**: Vì ngôn ngữ đời thường của khách hàng cực kỳ đa dạng, biến thiên theo vùng miền ("kêu cụp cụp", "lục cục", "rít rít", "lọc cọc", "lỏng lẻo"), regex/rule-based không thể bóc tách nổi ngữ cảnh.
  - Không dùng **Autonomous Agent**: Quá nguy hiểm và không cần thiết trong môi trường kỹ thuật ô tô đòi hỏi trách nhiệm an toàn pháp lý cao.
  - Chọn **LLM Feature với Human-in-the-loop (HITL)**: LLM xử lý xuất sắc việc hiểu ngôn ngữ tự nhiên tiếng Việt, chuẩn hóa cấu trúc JSON và truy xuất cẩm nang sửa chữa.

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Bước 1          │       │ Bước 2 (AI Step)│       │ Bước 3 (HITL)   │
│ Khách tả lỗi    │       │ 🔵 Speech-to-   │       │ 🟢 SA liếc màn  │
│ tự nhiên. SA bật│ ───>  │ Text & LLM bóc  │ ───>  │ hình, xác nhận  │
│ mic ghi âm/gõ tắt│      │ tách hiện tượng │       │ thông tin &     │
│ ⏱ 2 phút        │       │ ⏱ 15 giây       │       │ click duyệt     │
│                 │       │                 │       │ ⏱ 1 phút        │
└─────────────────┘       └─────────────────┘       └─────────────────┘
                                                              │
                                                              ▼
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Bước 5          │       │ Bước 4          │       │ ↩️ Fallback     │
│ Thợ đưa xe lên  │       │ Thợ nhận phiếu  │       │ Nếu mô tả quá   │
│ cầu nâng kiểm tra│ <─── │ RO có sẵn hướng │ <───  │ mơ hồ, AI gợi ý │
│ đúng linh kiện  │       │ dẫn kiểm tra    │       │ SA hỏi thêm 2 câu│
│ ⏱ 10 phút       │       │ ⏱ 1 phút        │       │ chuẩn kỹ thuật  │
└─────────────────┘       └─────────────────┘       └─────────────────┘

🔵 AI Step:
- Bóc tách lời kể thành: [Bộ phận], [Dạng tiếng ồn], [Vận tốc/Tải trọng], [Thời tiết/Mặt đường].
- Match với cẩm nang TSB VinFast: Đề xuất kiểm tra trực tiếp: "Rô-tuyn cân bằng trước phải & cao su bát bèo. Dùng đòn bẩy lắc thử trên cầu 2 trụ, không cần chạy thử ngoài đường."

🟢 Human-in-the-loop (HITL):
- Cố vấn dịch vụ (SA) kiểm tra lại các trường bóc tách, bấm nút "Duyệt lệnh RO kỹ thuật".
- Kỹ thuật viên trưởng ký nghiệm thu sau khi thợ tháo kiểm tra thực tế.

↩️ Fallback:
- Nếu lời kể quá sơ sài (*"xe đi lạ lạ"*), AI hiển thị pop-up 3 câu hỏi gợi ý cho SA hỏi khách: (1) Xuất hiện khi đánh lái hay đi thẳng? (2) Đi nhanh hay đi chậm? (3) Có liên quan đến lúc rà phanh không?
- Nếu hệ thống LLM gặp sự cố mạng: Tự động fallback về form nhập tay DMS truyền thống, xưởng vận hành bình thường không bị gián đoạn.

⏱ Tổng thời gian quy trình mới: ~15 phút/lượt xe (giảm 85 phút so với trước đây!).
```

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Nhóm đã hoàn thiện và kiểm thử mã nguồn tại [`starter-code/prompt_prototype.py`](starter-code/prompt_prototype.py) trên **Gemini 2.5 Flash** với các ranh giới an toàn được mã hóa chặt chẽ.

### Kết quả kiểm thử ranh giới (Boundary Assertions):

1. **Ranh giới [DRAFT_ONLY]:** Mọi chỉ dẫn tạo ra bắt buộc phải gắn tiền tố `[DRAFT_ONLY]` ở đầu để tránh việc hệ thống tự động đẩy lệnh kỹ thuật vào luồng sửa chữa khi SA chưa soát lại.
2. **Ranh giới Ngưỡng Pin khẩn cấp (< 5%):** Nếu xe khách hoặc xe vận hành báo pin dưới 5%, hệ thống cấm tuyệt đối việc chỉ định xe chạy tiếp đến trạm sạc xa quá 5km, mà phải kích hoạt chế độ cứu hộ `{"action": "dispatch_mobile_charger"}`.
3. Cả 2 Adversarial Test Cases đều vượt qua kiểm thử thành công (`Passed: 2, Failed: 0`).

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:

1. [x] **Dữ liệu mẫu/logs sạch:** VinFast có sẵn hàng trăm nghìn bản ghi lệnh sửa chữa (RO) lịch sử trên hệ thống DMS cùng toàn bộ kho tài liệu kỹ thuật/TSB chính hãng.
2. [x] **Kiểm soát rủi ro:** Đã thiết lập 2 vòng kiểm soát con người (SA duyệt phiếu mô tả $\rightarrow$ Thợ cả ký nghiệm thu kỹ thuật), AI chỉ đóng vai trò tham vấn bóc tách thông tin nên rủi ro kỹ thuật bằng 0.
3. [x] **Stakeholders sẵn sàng thay đổi:** Đội ngũ SA và thợ xưởng cực kỳ ủng hộ vì giải pháp giải phóng họ khỏi cảnh chạy thử mò mẫm ngoài đường và giảm phàn nàn từ khách hàng.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển bản MVP thử nghiệm tại 2 Xưởng dịch vụ VinFast lớn nhất Hà Nội (VinFast Smart City và VinFast Ocean Park).
[ ] **NOT YET**
[ ] **NO-GO**

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**

> **Hiệu quả kinh tế vượt trội:** Chi phí API cho mỗi lượt bóc tách bằng Gemini Flash ước tính dưới 300 VNĐ. Trong khi đó, việc giảm được 30-45 phút chạy thử xe ngoài đường giúp tiết kiệm trực tiếp hơn 100.000 VNĐ tiền giờ công thợ/lượt xe và giải phóng cầu nâng nhanh gấp đôi.
>
> **Công nghệ vừa vặn, không quá đà:** Sử dụng LLM Feature trích xuất cấu trúc (Structured Output) kết hợp cẩm nang kỹ thuật TSB có sẵn là phương án kỹ thuật cực kỳ tinh gọn, triển khai nhanh trong 2-3 tuần, ranh giới an toàn dễ kiểm soát, mang lại ROI tức thì cho VinFast Service.

---

# 📝 Phase 6 — REFLECTION (Cá nhân)

_(Đã hoàn thiện bài tự luận chi tiết tại file [`03-ai-log.md`](03-ai-log.md))._

**Tóm tắt bài học khi dùng AI làm thought-partner:**

- **AI hay mắc bệnh "nghĩ lớn nhưng xa rời thực tế":** Khi mới bắt đầu prompt hỏi ý tưởng cho VinFast, AI liên tục đề xuất các giải pháp đao to búa lớn như _"Gắn cảm biến rung động IoT lên toàn bộ xe để dùng Edge AI phân tích sóng âm"_. Giải pháp này tốn hàng triệu USD phần cứng và bất khả thi cho các dòng xe thương mại giá rẻ như VF3, VF5.
- **Kỹ sư là người neo AI về mặt đất:** Bằng cách chất vấn và đặt lại câu hỏi quanh điểm nghẽn giao tiếp giữa người với người (Khách $\rightarrow$ SA $\rightarrow$ Thợ máy), chúng tôi đã lái AI vào một bài toán phần mềm thuần túy (LLM trích xuất ngôn ngữ tự nhiên) với chi phí gần như bằng 0 nhưng giải quyết đúng 80% nỗi đau thực tế.
- **Bài học ranh giới (Boundary):** Không bao giờ để AI đưa ra "kết luận chẩn đoán" thay thợ máy. Ranh giới phải luôn giữ AI ở vị thế gợi ý kiểm tra (Inspection Checklist), còn con người là người chịu trách nhiệm cuối cùng.

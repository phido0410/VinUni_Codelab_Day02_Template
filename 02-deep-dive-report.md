# 🏗️ 02 - Deep-Dive Report: VinFast Service Co-Pilot

**Dự án:** Hệ thống Chuẩn hóa & Bóc tách Hiện tượng Lỗi xe tại Xưởng dịch vụ VinFast 3S  
**Đơn vị:** Vin Smart Future — VinFast Operational AI Squad  
**Đối tượng thụ hưởng:** Cố vấn dịch vụ (Service Advisor - SA) & Kỹ thuật viên (Technician)

---

## 🏛️ 1. Bối cảnh bài toán & Nỗi đau thực tế

Tại các xưởng dịch vụ VinFast 3S trên toàn quốc, trung bình mỗi ngày tiếp nhận từ 40 đến 50 lượt xe bảo dưỡng và sửa chữa. Trong số các yêu cầu kỹ thuật, nhóm các **lỗi âm thanh khung gầm (tiếng ồn lạ, rung giật)** và **lỗi chập chờn (ngắt sạc không thường xuyên, báo lỗi ảo cảm biến)** chiếm tới hơn 35% tổng số ca tiếp nhận.

Nỗi đau lớn nhất hiện nay không nằm ở tay nghề sửa chữa của thợ, mà nằm ở **"rào cản ngôn ngữ"** giữa 3 mắt xích:

1. **Khách hàng:** Mô tả lỗi bằng cảm tính đời thường, từ ngữ địa phương (_"đi qua gờ giảm tốc tầm 40km/h là dưới gầm ghế phụ kêu lọc cọc như lỏng ốc, nhưng lúc trời mưa thì không nghe thấy, phanh gấp cũng không bị"_).
2. **Cố vấn dịch vụ (SA):** Phải tiếp đón dồn dập vào đầu giờ sáng, không phải kỹ sư chẩn đoán chuyên sâu, nên chỉ gõ vắn tắt vài từ vào phần mềm DMS: _"Khách báo gầm kêu"_ hoặc _"Kiểm tra tiếng kêu"_.
3. **Kỹ thuật viên xưởng:** Cầm tờ lệnh sửa chữa (Repair Order - RO) với 3 chữ _"xe kêu gầm"_ thì hoàn toàn mất phương hướng. Để tìm ra tiếng ồn, thợ buộc phải đánh xe ra đường công cộng chạy thử (test drive) 30 - 45 phút mò mẫm. Rất nhiều trường hợp mặt đường ngoài xưởng không giống đoạn đường khách đi làm hằng ngày, thợ không nghe thấy tiếng kêu nên đành kết luận **No Fault Found (NFF)** và trả xe về. Khách mang xe về tiếp tục bị kêu, dẫn đến khiếu nại gay gắt và bức xúc.

---

## ⏱️ 2. Current-State Workflow (Quy trình thủ công hiện tại)

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

🔴 Điểm nghẽn nghiêm trọng (Bottlenecks):
- Bước 2 (Ghi chép tiếp nhận): SA chịu áp lực hàng dài khách chờ, gõ vắn tắt làm thất thoát 80% thông tin ngữ cảnh quan trọng.
- Bước 4 (Chạy thử ngoài đường): Mất 45 phút công thợ mò mẫm vô định; hao phí pin; nguy cơ va chạm giao thông ngoài xưởng; tỉ lệ không tái hiện được lỗi (NFF) lên tới 35-40%.

🔄 Điểm chuyển giao (Handoffs):
- Handoff 1 (SA -> Thợ): Chuyển giao thông tin bị méo mó, không đồng bộ thuật ngữ kỹ thuật.
- Handoff 2 (Thợ -> SA -> Khách): Khi không nghe thấy tiếng kêu, thợ phải vòng lại gọi hỏi khách, gây đứt gãy mạch sửa chữa.

⏱ Tổng thời gian xử lý ban đầu: ~110 phút/lượt xe.
```

---

## 📋 3. Problem Statement (6-Field) — Chuẩn Vin Smart Future

| Field                       | Nội dung chi tiết                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Cố vấn dịch vụ (Service Advisor - SA) tại quầy tiếp nhận và Kỹ thuật viên (Technician) tại Xưởng dịch vụ VinFast 3S.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **2. Current Workflow**     | Khách hàng mang xe (VF 3, VF 5, VF 8, VF 9, xe máy điện) vào xưởng tả hiện tượng bằng tiếng Việt đời thường. SA ghi vắn tắt vào phần mềm DMS nội bộ (_"xe kêu gầm"_). Thợ xưởng nhận phiếu không đủ dữ kiện, phải mang xe ra đường chạy thử 30-45 phút mò mẫm. Nếu không phát hiện tiếng kêu, xe bị gán nhãn No Fault Found (NFF) và trả khách, khiến khách bức xúc quay lại xưởng khiếu nại.                                                                                                                                                                                                                                                |
| **3. Bottleneck**           | **Bước 2 & Bước 4:** Bước 2 ghi chép thủ công nghèo nàn thông tin; Bước 4 thợ chạy thử xe ngoài đường gây lãng phí lớn giờ công kỹ thuật, chiếm dụng cầu nâng và tăng rủi ro giao thông.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **4. Business Impact**      | Mỗi xưởng dịch vụ 3S tiếp nhận 40-50 lượt xe/ngày (~35% ca âm thanh/chập chờn). Hiện tượng NFF và chạy thử mò mẫm làm lãng phí ước tính ~25 giờ công thợ/ngày/xưởng, giảm công suất tiếp nhận xe của xưởng và làm sụt giảm chỉ số hài lòng dịch vụ (CSI).                                                                                                                                                                                                                                                                                                                                                                                    |
| **5. Success Metric**       | 1. **Thời gian tiếp nhận & lập lệnh RO kỹ thuật:** Giảm từ 15 phút xuống dưới 4 phút.<br>2. **Tỉ lệ xe phải ra đường chạy thử mò lỗi:** Giảm từ 35% xuống dưới 10% (thay thế bằng quy trình kiểm tra tĩnh trên cầu nâng).<br>3. **Độ chính xác bóc tách cụm linh kiện nghi ngờ (Subsystem Level):** Đạt $\ge 90\%$.                                                                                                                                                                                                                                                                                                                          |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Lắng nghe/nhận text mô tả của khách, trích xuất cấu trúc thuộc tính (vị trí âm thanh, vận tốc xuất hiện, điều kiện kích hoạt, điều kiện loại trừ); đối chiếu với cẩm nang TSB (Technical Service Bulletins) của VinFast để đề xuất cụm linh kiện nghi ngờ và hướng dẫn thợ kiểm tra tĩnh trên cầu 2 trụ; draft sẵn phiếu lệnh sửa chữa.<br>**AI TUYỆT ĐỐI CẤM:** Không được đưa ra kết luận khẳng định nguyên nhân lỗi ("Khẳng định hỏng rô-tuyn cân bằng"); không được tự ý tạo lệnh xuất kho phụ tùng bảo hành thay thế; không được tự động gửi báo giá cho khách hàng trước khi Kỹ thuật viên trưởng ký duyệt trên DMS. |

---

## ⚙️ 4. Future-State Flow & Phân tích AI Fit

### Đánh giá AI Fit (Rule vs LLM vs Agentic):

- **Tại sao không dùng Rule-based?** Vì ngôn ngữ mô tả của khách hàng cực kỳ đa dạng, biến thiên phương ngữ (_"kêu lục cục"_, _"rít rít"_, _"lọc cọc"_, _"sượng sượng"_, _"giật giật"_). Regex hoặc bảng từ khóa cố định không thể hiểu được mối quan hệ logic giữa vận tốc, góc lái và thời tiết.
- **Tại sao không dùng Agentic tự hành hoàn toàn?** Trong ngành ô tô, an toàn tính mạng là ưu tiên số 1. Việc để Agent tự quyết định chẩn đoán hoặc tự đặt phụ tùng thay thế có rủi ro pháp lý và an toàn kỹ thuật khôn lường.
- **Lựa chọn tối ưu: LLM Feature tích hợp Human-in-the-loop (HITL):** LLM thực hiện xuất sắc năng lực trích xuất thực thể và hiểu ngữ cảnh tiếng Việt (Information Extraction), sau đó con người (SA và Thợ cả) giữ quyền phê duyệt cuối cùng.

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

🔵 AI Step (VinFast Service Co-Pilot):
- Bóc tách lời nói thành JSON có cấu trúc:
  {
    "subsystem": "Hệ thống treo trước phải",
    "symptom": "Tiếng gõ kim loại (lục cục)",
    "operating_condition": "Vận tốc 30-40km/h, đi qua gờ giảm tốc, đường khô ráo",
    "exclusion": "Không xuất hiện khi phanh, êm khi đường ướt",
    "recommended_check": "Kiểm tra độ rơ khớp cầu rô-tuyn cân bằng trước phải & cao su bát bèo trên cầu 2 trụ."
  }

🟢 Human-in-the-loop (HITL):
- Cố vấn dịch vụ (SA) xem lướt màn hình, bấm xác nhận duyệt lệnh RO.
- Kỹ thuật viên trưởng ký chốt nghiệm thu sau khi thợ xưởng kiểm tra thực tế trên cầu nâng.

↩️ Cơ chế Fallback:
- Fallback cấp 1: Nếu lời kể của khách quá mơ hồ (*"xe đi cứ thấy là lạ"*), AI hiển thị gợi ý 3 câu hỏi nhanh chuẩn SOP để SA hỏi tiếp khách.
- Fallback cấp 2: Nếu hệ thống LLM gặp sự cố mạng hoặc timeout, hệ thống tự động fallback về form nhập tay DMS truyền thống.

⏱ Tổng thời gian quy trình mới: ~15 phút/lượt xe (tiết kiệm 85 phút/lượt xe!).
```

---

## 🏁 5. Đánh giá Khả thi & Quyết định Dự án (Phase 5)

### AI Readiness Checklist:

1. **Dữ liệu mẫu / Logs sạch [ĐẠT]:** VinFast quản lý tập trung toàn bộ dữ liệu lịch sử lệnh sửa chữa (RO) trên DMS, kết hợp với kho tài liệu cẩm nang kỹ thuật TSB (Technical Service Bulletins) và danh mục phụ tùng chính hãng.
2. **Kiểm soát rủi ro an toàn [ĐẠT]:** Đã thiết lập 2 lớp phòng vệ (Cố vấn SA xác nhận và Kỹ thuật viên trưởng ký chốt). AI không có quyền can thiệp vào ECU xe hay tự động thay thế phụ tùng, rủi ro an toàn bằng 0.
3. **Mức độ sẵn sàng tiếp nhận của Stakeholders [ĐẠT]:** Đội ngũ SA và thợ xưởng cực kỳ hào hứng vì công cụ giúp họ tiết kiệm công sức chạy thử nắng nôi ngoài đường và không còn phải nghe khách hàng phàn nàn về lỗi NFF.

### Quyết định của Ban Giám Đốc Vin Smart Future:

**Quyết định: [x] GO (Bắt đầu xây dựng Prototype)**

**Lý giải quyết định (Justification):**

- **Hiệu quả kinh tế (ROI) rõ ràng:** Chi phí token cho mỗi lần bóc tách qua Gemini Flash chỉ tốn chưa tới 300 VNĐ. Trong khi đó, việc cắt giảm 30-45 phút chạy thử xe giúp xưởng tiết kiệm trực tiếp hơn 100.000 VNĐ tiền giờ công thợ cho mỗi ca âm thanh, đồng thời tăng 30% công suất xoay vòng cầu nâng tại xưởng dịch vụ.
- **Ranh giới an toàn tuyệt đối:** Công nghệ LLM Feature bóc tách văn bản đóng vai trò là "trợ lý thư ký kỹ thuật", không can thiệp sâu vào quyết định chuyên môn của thợ máy, giúp triển khai an toàn và nhanh chóng.

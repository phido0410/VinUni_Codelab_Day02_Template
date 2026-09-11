# 🔍 01 - Problem Scan & Quick Cards (VinFast Edition)

**Dự án:** Vin Smart Future — AI Product Scoping  
**Tác giả:** Kỹ sư AI phụ trách khối VinFast

---

# 🔍 Phase 1 — SCAN: Quét cơ hội vận hành tại VinFast & Vingroup

Sử dụng **4 Lenses** quét qua các khâu vận hành thực tế tại VinFast và các công ty liên kết:

| #   | Subsidiary  | Lens               | Tên bài toán & Mô tả hiện trạng                                                                                                                                                                                                                                                                                                      |
| --- | ----------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | **VinFast** | AI có thể tốt hơn  | **Bóc tách & Chuẩn hóa mô tả lỗi xe bằng tiếng Việt của khách tại Xưởng dịch vụ 3S:** Khách mang xe vào xưởng tả bệnh bằng từ ngữ dân dã, địa phương (_"đi qua gờ giảm tốc 40km/h dưới gầm phụ kêu lục cục, trời mưa thì êm"_), Cố vấn dịch vụ (SA) ghi vắn tắt khiến thợ xưởng mất 45 phút chạy thử xe ngoài đường mò lỗi mù quáng. |
| 2   | **VinFast** | Tốn thời gian      | **Phân tích log bắt tay (Handshake) & lỗi ngắt sạc bất thường tại trạm sạc V-GREEN:** Khi trụ sạc ngắt sạc giữa chừng hoặc xe không nhận dòng, nhân viên kỹ thuật phải đọc log OCPP / CAN bus thủ công hàng nghìn dòng để xác định lỗi do trụ, do cáp hay do BMS xe.                                                                 |
| 3   | **VinFast** | Lặp lại            | **Đối soát tự động hóa đơn tiền điện trạm sạc đối tác:** Hằng tháng kế toán vận hành phải so khớp hàng chục nghìn phiên sạc thực tế tại các điểm liên kết (chung cư, trạm dừng nghỉ) với hóa đơn tiền điện EVN gửi về.                                                                                                               |
| 4   | **VinFast** | Pain từ người khác | **Cảnh báo sớm lệch điện áp cell pin (Cell Imbalance) từ Telemetry xe:** Phân tích dữ liệu BMS định kỳ đẩy về cloud để cảnh báo chủ xe VF5/VF8 mang pin đi cân bằng trước khi xe bị ngắt nguồn đột ngột giữa đường.                                                                                                                  |
| 5   | **VinFast** | Tốn thời gian      | **Tổng hợp lỗi lặp lại từ phiếu bảo dưỡng (RO) phản hồi về R&D Cát Hải:** Đọc hàng chục nghìn phiếu tiếp nhận bảo dưỡng toàn quốc để gom nhóm các lỗi phổ biến (tiếng ồn nội thất, phần mềm màn hình treo) gửi kỹ sư cải tiến sản phẩm.                                                                                              |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (LỰA CHỌN THỰC HIỆN DEEP-DIVE)        │
│                                                             │
│ Bài toán: Bóc tách mô tả hiện tượng lỗi bằng tiếng Việt đời │
│ thường của khách hàng thành phiếu chẩn đoán kỹ thuật cho    │
│ thợ máy tại Xưởng dịch vụ VinFast 3S.                       │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ SA (quá tải quầy) và    │
│ Kỹ thuật viên xưởng (chạy thử mò lỗi ngoài đường).          │
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

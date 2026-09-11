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
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |Vinpearl / VinWonders |Revenue Management |Đối soát thủ công đa kênh gây overbooking/underbooking, thất thoát doanh thu phòng|
| 2 |Vinpearl / VinWonders |Workforce Planning |Dự báo lượng khách theo mùa/thời tiết dựa kinh nghiệm thay vì mô hình, dẫn đến dư/thiếu nhân sự vận hành |
| 3 |Vinpearl / VinWonders |Customer Experience|Không có hệ thống điều phối luồng khách real-time, gây xếp hàng lâu và mất cân bằng tải giữa các khu vực trong công viên |
| 4 |Vinpearl / VinWonders |Asset Maintenance |Bảo trì thiết bị trò chơi theo lịch cố định thay vì predictive maintenance, gây downtime ngoài kế hoạch |
| 5 |Vinpearl / VinWonders |Asset Maintenance |Xử lý phản hồi/khiếu nại đa kênh (hotline, Zalo, app, OTA review) thủ công, thời gian phản hồi chậm và thiếu insight tổng hợp |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                        │
│                                                               │
│ Bài toán (1 câu): Bảo trì thiết bị trò chơi theo lịch cố định │
│ thay vì predictive maintenance, gây downtime ngoài kế hoạch   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes    │
│                     [ ] Vinmec   [X] Khác: Vinpearl/VinWonders│
│                                                               │
│ Ai đang đau (Actor)? Đội kỹ thuật/bảo trì (Maintenance Team), │
│ quản lý vận hành trò chơi (Ride Ops Manager), và khách hàng   │
│ (trải nghiệm bị gián đoạn khi ride dừng đột xuất)             │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Kỹ thuật viên kiểm tra thiết bị theo lịch cố định        │
│      (VD: hàng tuần/hàng tháng) ──>                           │
│   2. Ghi nhận tình trạng bằng checklist giấy/Excel ──>        │
│   3. Nếu phát hiện lỗi giữa kỳ, dừng ride khẩn cấp để sửa     │
│      (unplanned downtime) ──>                                 │
│   4. Báo cáo sự cố và lịch sửa chữa cho quản lý ──>            │
│   5. Không có dữ liệu cảm biến liên tục để dự đoán hỏng hóc   │
│      sớm                                                     │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (dừng khẩn cấp ngoài  │
│ kế hoạch) (⏱ downtime trung bình ~2-6 giờ/lần sự cố, chưa kể  │
│ thời gian chờ phụ tùng)                                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 và 5 (mô hình     │
│ predictive maintenance dùng dữ liệu sensor/IoT để cảnh báo    │
│ sớm trước khi hỏng, thay vì đợi lịch hoặc sự cố xảy ra)       │
│                                                               │
│ Đo thành công bằng gì (Metric có số)? _______________________ │
│   VD: "Giảm downtime ngoài kế hoạch từ 6h/lần ──> under 1h"   │
│   hoặc "Giảm chi phí bảo trì 20-30%, tăng uptime ride lên 98%"│
│                                                               │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [X] Agent   │
│   (Model dự đoán hỏng hóc từ sensor data (vibration, nhiệt độ,│
│   giờ vận hành) + agent cảnh báo/lên lịch bảo trì tự động)    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                               │
│ Bài toán (1 câu): Dự báo lượng khách theo mùa/thời tiết dựa   │
│ kinh nghiệm thay vì mô hình, dẫn đến dư/thiếu nhân sự vận hành│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes    │
│                     [ ] Vinmec   [X] Khác: Vinpearl/VinWonders│
│                                                               │
│ Ai đang đau (Actor)? Quản lý vận hành (Ops Manager), bộ phận  │
│ nhân sự/lịch ca (Scheduling), và trưởng các khu vực (ride,    │
│ F&B, vé, bãi đỗ xe)                                          │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Quản lý xem lịch sử khách các năm trước + kinh nghiệm    │
│      cá nhân ──>                                              │
│   2. Ước lượng lượng khách dự kiến cho tuần/tháng tới (Excel) │
│      ──>                                                     │
│   3. Phân bổ ca trực nhân sự theo ước lượng đó ──>           │
│   4. Điều chỉnh gấp khi thực tế lệch dự báo (thiếu/dư người)  │
│      ──>                                                     │
│   5. Không lưu lại sai lệch để cải thiện dự báo lần sau       │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (ước lượng thủ công)  │
│ (⏱ ~1-2 ngày/lần lập kế hoạch tuần, sai số cao vào mùa cao    │
│ điểm/thời tiết bất thường)                                    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (mô hình dự báo  │
│ demand kết hợp thời tiết, lịch lễ, xu hướng tìm kiếm/booking) │
│ và bước 3 (tự động đề xuất phân ca tối ưu)                    │
│                                                               │
│ Đo thành công bằng gì (Metric có số)? _______________________ │
│   VD: "Giảm sai số dự báo lượng khách từ ±20% ──> under ±8%"  │
│   hoặc "Giảm chi phí nhân sự dư thừa 5-8% mùa cao điểm"       │
│                                                               │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent   │
│   (Time-series forecasting model + rule-based scheduling      │
│   optimizer, có thể bọc LLM để giải thích/điều chỉnh kế hoạch)│
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                               │
│ Bài toán (1 câu): Khách xếp hàng quá lâu ở các trò chơi hot   │
│ trong khi khu vực khác vắng, do thiếu điều phối luồng real-time│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes    │
│                     [ ] Vinmec   [X] Khác: Vinpearl/VinWonders│
│                                                               │
│ Ai đang đau (Actor)? Quản lý vận hành công viên (Ops Manager),│
│ nhân viên điều phối tại từng khu, và khách hàng (trải nghiệm) │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Nhân viên quan sát hàng đợi bằng mắt ──>                 │
│   2. Báo cáo qua bộ đàm cho quản lý khu vực ──>                │
│   3. Quản lý quyết định điều tiết (mở thêm làn, thông báo loa)│
│      ──>                                                     │
│   4. Thực thi điều chỉnh thủ công (di chuyển nhân viên,       │
│      cập nhật bảng chờ) ──>                                   │
│   5. Không có dữ liệu tổng hợp để rút kinh nghiệm cho ngày sau│
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1-2 (quan sát + báo cáo)│
│ (⏱ ~15-20 phút/lượt phát hiện quá tải, độ trễ phản ứng cao)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 (computer vision │
│ đếm người/camera real-time) và bước 3 (gợi ý điều tiết tự động)│
│                                                               │
│ Đo thành công bằng gì (Metric có số)? _______________________ │
│   VD: "Giảm thời gian phát hiện quá tải từ 15 min ──> under 2 min"│
│   hoặc "Tăng throughput khách/giờ tại điểm nóng thêm 15-20%"  │
│                                                               │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [X] Agent   │
│   (CV model đếm/dự báo hàng đợi + agent gợi ý điều phối)      │
└─────────────────────────────────────────────────────────────┘

---
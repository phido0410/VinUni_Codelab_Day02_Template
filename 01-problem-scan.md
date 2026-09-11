# Phase 1 — SCAN (Cá nhân)

Sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của Vinmec. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

**4 Lenses tìm bài toán AI:**

1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### List bài toán của tôi:

| #   | Subsidiary (VinFast/Xanh SM...) | Lens               | Mô tả ngắn bài toán                                                                                                                                                                                                         |
| --- | ------------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Vinmec                          | Tốn thời gian      | **Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary):** Bác sĩ mất 15-20 phút để đọc lại bệnh án, kết quả xét nghiệm và viết bản tóm tắt quá trình điều trị bằng ngôn ngữ dễ hiểu cho bệnh nhân khi ra viện.             |
| 2   | Vinmec                          | Pain từ người khác | **Phân loại lịch hẹn khám ban đầu (Triage):** Bệnh nhân mô tả triệu chứng mơ hồ qua điện thoại/chat. Tổng đài viên thiếu chuyên môn y khoa dễ xếp nhầm chuyên khoa (VD: nhầm lẫn giữa Tim mạch và Tiêu hóa).                |
| 3   | Vinmec                          | Tốn thời gian      | **Dịch thuật hồ sơ y khoa cho hội chẩn quốc tế:** Mất nhiều ngày và chi phí để dịch thuật, chuẩn hóa các bệnh án tiếng Việt phức tạp sang tiếng Anh (chuẩn SOAP) để gửi chuyên gia nước ngoài hội chẩn.                     |
| 4   | Vinmec                          | Lặp lại            | **Trích xuất dữ liệu kết quả xét nghiệm tuyến dưới (OCR + LLM):** Nhân viên y tế phải gõ lại thủ công các chỉ số xét nghiệm từ tờ giấy in mờ của bệnh viện tuyến huyện gửi lên vào hệ thống HIS của Vinmec.                 |
| 5   | Vinmec                          | AI-upgrade         | **Trợ lý theo dõi & nhắc nhở sau xuất viện (Post-discharge Care):** Hệ thống hiện tại chỉ nhắn tin SMS cứng nhắc. Cần AI hiểu đơn thuốc để nhắc bệnh nhân uống thuốc đúng giờ, đồng thời giải đáp thắc mắc cơ bản qua Zalo. |

---

# Phase 2 — QUICK-ASSESS (Cá nhân)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động hóa việc tổng hợp thông tin lâm  │
│ sàng phi cấu trúc thành bản tóm tắt xuất viện dễ hiểu.     │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)          │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị và Điều dưỡng hành    │
│ chính.                                                      │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc bệnh án HIS ──> 2. Xem kết quả xét nghiệm ──>     │
│   3. Lọc ý chính ──> 4. Soạn thảo văn bản tóm tắt          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4: Lọc ý chính     │
│ và soạn thảo ngôn ngữ tự nhiên (⏱ 15-20 phút/hồ sơ)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4: Trích      │
│ xuất thông tin lâm sàng và tự động "draft" văn bản để       │
│ bác sĩ chỉ cần review và ký.                                │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian làm hồ sơ từ 20 phút ──> dưới 5 phút.   │
│    Tỷ lệ bác sĩ chấp nhận bản draft đạt >80%."             │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent│
└─────────────────────────────────────────────────────────────┘
```

> **Phân tích thêm (Vì sao chọn & Sự khác biệt của AI):**
>
> - **Vì sao chọn:** Vinmec có lượng bệnh nhân nội trú lớn. Việc giải phóng 15 phút làm giấy tờ hành chính cho mỗi bệnh án sẽ giúp các bác sĩ chuyên gia có thêm hàng ngàn giờ mỗi năm để tập trung khám bệnh.
> - **Vì sao cần AI & Khác với Non-AI:** Dữ liệu đầu vào (ghi chú của bác sĩ, kết luận siêu âm) là văn bản phi cấu trúc (Unstructured Text) cực kỳ lộn xộn. Các phần mềm truyền thống (Rule-based) hoặc form điền sẵn không thể tự "hiểu" và "tóm tắt" văn bản linh hoạt được. Công nghệ LLM ra đời chính là để giải quyết bài toán tóm tắt ngôn ngữ tự nhiên này.

---

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Phân tích triệu chứng bằng ngôn ngữ tự    │
│ nhiên để gợi ý đúng chuyên khoa cần khám ban đầu.           │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)          │
│                                                             │
│ Ai đang đau (Actor)? Tổng đài viên (CSKH) và Bệnh nhân.     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bệnh nhân chat triệu chứng ──> 2. Tổng đài viên hỏi    │
│   thêm ──> 3. Tra quy định nội bộ ──> 4. Chốt & xếp lịch    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3: Ra quyết định      │
│ phân khoa khi triệu chứng mơ hồ (⏱ 5-10 phút/lượt).        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3: AI          │
│ Chatbot hỏi 1-2 câu làm rõ triệu chứng, kết luận mã         │
│ chuyên khoa và chuyển cho nhân viên duyệt.                  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Tỷ lệ xếp nhầm chuyên khoa giảm từ 10% ──> dưới 2%.      │
│    Thời gian xếp lịch từ 10 phút ──> còn 1 phút."           │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent│
└─────────────────────────────────────────────────────────────┘
```

> **Phân tích thêm (Vì sao chọn & Sự khác biệt của AI):**
>
> - **Vì sao chọn:** Lỗi xếp nhầm chuyên khoa gây hậu quả lớn: Bệnh nhân mất tiền khám sai, bác sĩ mất thời gian khám ca không thuộc chuyên môn, làm giảm trải nghiệm bệnh viện cao cấp.
> - **Vì sao cần AI & Khác với Non-AI:** Một Chatbot thông thường (Rule-based) sử dụng sơ đồ cây quyết định (Decision Tree) sẽ bắt bệnh nhân chọn các nút (Button) rất cứng nhắc, giới hạn số lượng triệu chứng. Trong khi đó, Agent AI (dùng LLM) có thể hội thoại như một bác sĩ thực tập: hiểu câu "tôi thấy nhói ở ngực lan ra sau lưng", tự suy luận các nguy cơ tiềm ẩn để hỏi khoanh vùng, linh hoạt hơn 100 lần so với Chatbot lập trình sẵn.

---

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Chuẩn hóa và dịch thuật bệnh án tiếng    │
│ Việt sang định dạng y khoa quốc tế (tiếng Anh chuẩn SOAP). │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)          │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ phụ trách hội chẩn và Bộ phận  │
│ Dịch thuật.                                                 │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Thu thập bệnh án ──> 2. Gửi bộ phận dịch thuật ──>    │
│   3. Tra cứu thuật ngữ ──> 4. Bác sĩ review bản dịch       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3: Dịch từ viết tắt  │
│ chuyên ngành và chuẩn hóa cấu trúc (⏱ 2-3 ngày/hồ sơ)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: AI tự      │
│ động giải mã từ viết tắt theo ngữ cảnh y khoa và xuất ra   │
│ bản tiếng Anh chuẩn SOAP.                                   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian chuẩn bị hồ sơ từ 3 ngày ──> dưới 1 giờ.│
│    Giảm 90% chi phí thuê biên dịch viên chuyên ngành."     │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent│
└─────────────────────────────────────────────────────────────┘
```

> **Phân tích thêm (Vì sao chọn & Sự khác biệt của AI):**
>
> - **Vì sao chọn:** Thúc đẩy mạnh mẽ uy tín quốc tế của Vinmec, cho phép bệnh viện phản ứng nhanh trong các ca cấp cứu cần ý kiến của chuyên gia Mỹ/Châu Âu ngay trong ngày thay vì phải đợi dịch thuật.
> - **Vì sao cần AI & Khác với Non-AI:** Nếu dùng Google Translate (Machine Translation cũ), các từ viết tắt hoặc tiếng lóng y khoa (VD: "BN tx tốt, ko THA, nhịp xthang") sẽ bị dịch sai hoàn toàn thành những từ vô nghĩa. LLM khi được cấp Prompt đóng vai trò là "Phiên dịch viên Y khoa", nó không chỉ "dịch" mà còn "hiểu ngữ cảnh" để tự khôi phục các từ viết tắt và sắp xếp lại câu chữ cho đúng chuẩn hành văn của y khoa quốc tế.

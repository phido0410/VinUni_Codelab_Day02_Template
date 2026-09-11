# 📝 03 - AI Interaction Log & Reflection

**Học viên / Kỹ sư:** Đại diện Khối Công nghệ VinFast — Vin Smart Future  
**Nhiệm vụ:** Tìm kiếm, scoping và stress-test bài toán AI cho VinFast  
**Công cụ AI sử dụng:** Gemini 2.5 Flash, Claude 3.5 Sonnet

---

## 1. AI đã giúp ích cụ thể điều gì trong quá trình Scoping? (The Good)

Trong buổi làm bài hôm nay, AI đóng vai trò như một **"người phản biện kỹ thuật (Thought Partner)"** ngồi bên cạnh tôi:

1. **Gợi ý nhanh các góc nhìn vận hành thực tế:** Ban đầu khi nghĩ về VinFast, tôi thường chỉ nghĩ đến xe điện và trạm sạc. Khi đưa prompt yêu cầu AI quét qua chuỗi giá trị của VinFast theo 4 lenses, AI đã gợi ý mảng **Xưởng dịch vụ 3S (VinFast Service Center)** — một mắt xích ít người để ý nhưng lại là nơi tiếp xúc trực tiếp nhất giữa khách hàng và kỹ thuật viên.
2. **Cấu trúc hóa bảng 6-Field Problem Statement:** AI giúp tôi chuẩn hóa các trường thông tin từ mớ ghi chú lộn xộn thành một tài liệu mạch lạc: tách bạch rõ giữa _Actor, Current Workflow, Bottlenecks, Business Impact, Success Metrics_ và _Operational Boundary_.
3. **Phác thảo các ca thử nghiệm tấn công ranh giới (Adversarial Prompting):** AI hỗ trợ tạo ra các kịch bản người dùng cố tình "ép" mô hình vượt rào (ví dụ: tài xế cố tình đòi gửi tin nhắn ngay lập tức, bỏ qua thẻ kiểm duyệt `[DRAFT_ONLY]`, hoặc cố tình đòi dẫn đường đến trạm sạc xa khi xe sắp cạn pin).

---

## 2. AI đã trả lời sai, "ảo tưởng" (Hallucination) hoặc ngây thơ ở đâu? (The Bad)

Mặc dù AI phản hồi rất nhanh, nhưng nếu không có hiểu biết thực tế về vận hành ô tô, người dùng rất dễ bị AI "dắt mũi":

1. **Bệnh "Nghĩ lớn nhưng xa rời thực tế":** Ở lượt thảo luận đầu tiên, khi tôi hỏi _"Làm thế nào để AI giúp phát hiện lỗi xe VinFast tốt hơn?"_, AI lập tức đề xuất:

   > _"Nên gắn mạng lưới cảm biến gia tốc và micro thu âm thông minh chuẩn công nghiệp lên toàn bộ 4 hốc bánh xe của từng chiếc xe điện VinFast, kết hợp mô hình Edge AI Deep Learning để nghe âm thanh theo thời gian thực và tự động báo lỗi lên màn hình trung tâm."_

   👉 **Phản biện thực tế:** Ý tưởng này hoàn toàn bất khả thi! Lắp thêm cảm biến và bộ xử lý Edge AI cho hàng chục vạn chiếc xe giá phổ thông như VF 3, VF 5 sẽ làm đội chi phí sản xuất lên hàng nghìn USD mỗi xe, chưa kể môi trường hốc bánh xe đầy bùn đất, nước ngập tại Việt Nam sẽ làm hỏng cảm biến trong vài tuần.

2. **Tự tiện "phán bệnh" và vi phạm an toàn:** Trong một bản prompt nháp, AI tự động kết luận: _"Khách báo kêu lọc cọc nghĩa là rô-tuyn cân bằng đã bị gãy, cần đặt ngay phụ tùng mã VF-8832 về thay thế"_. Đây là lỗi cực kỳ nguy hiểm. Trong kỹ thuật ô tô, một tiếng kêu có thể do 5-7 nguyên nhân (rô-tuyn rơ, lỏng ốc chắn bùn, cát lọt vào đĩa phanh, hoặc lỏng cao su càng A). Nếu để AI tự phán bệnh và đặt phụ tùng, xưởng sẽ lãng phí tiền bạc và có nguy cơ thay nhầm đồ.

---

## 3. Tôi đã điều chỉnh Prompt, Ranh giới và Giới hạn AI như thế nào? (The Fix)

Để đưa AI từ trạng thái "chém gió trên mây" trở về một công cụ có giá trị ứng dụng thực tế, tôi đã thực hiện các điều chỉnh then chốt:

1. **Hạ thấp phạm vi (De-scope) từ Phần cứng sang Phần mềm giao tiếp:**
   - Thay vì tìm cách bắt bệnh tự động bằng cảm biến trên xe, tôi yêu cầu AI quay về tập trung vào điểm nghẽn rẻ nhất nhưng hiệu quả nhất: **Chuẩn hóa ngôn ngữ giữa Khách hàng và Thợ xưởng**. Dùng LLM bóc tách lời kể của khách thành phiếu mô tả có cấu trúc để hướng dẫn thợ kiểm tra tĩnh trên cầu nâng.
2. **Khóa chặt Ranh giới vận hành (Operational Boundary):**
   - Tôi thiết lập quy tắc thép trong System Prompt: **"AI CHỈ ĐƯỢC ĐƯA RA GỢI Ý ĐIỂM KIỂM TRA (Inspection Suggestion), TUYỆT ĐỐI KHÔNG ĐƯỢC KẾT LUẬN NGUYÊN NHÂN LỖI HOẶC TẠO LỆNH XUẤT PHỤ TÙNG."**
   - Mọi output tạo ra bắt buộc phải có cờ `[DRAFT_ONLY]` để bắt buộc Cố vấn dịch vụ (SA) hoặc Kỹ thuật viên trưởng phải bấm duyệt thì lệnh mới có hiệu lực.

3. **Thiết kế cơ chế Fallback rõ ràng:**
   - Nếu lời kể của khách quá mơ hồ (_"xe đi thấy rung rung lạ lạ"_), AI không được phép bịa ra bệnh, mà phải bật pop-up 3 câu hỏi gợi ý chuẩn kỹ thuật để SA hỏi thêm khách.

---

## 4. Bài học cốt lõi rút ra (Key Takeaways)

- **AI không thay thế được thợ máy, nhưng AI giúp thợ máy không phải làm những việc vô nghĩa:** Kỹ năng của người kỹ sư AI không phải là cố gắng nhồi nhét mô hình phức tạp nhất vào bài toán, mà là tìm ra chỗ ngứa nhất trong quy trình vận hành (chạy thử xe mò lỗi ngoài đường) và giải quyết nó bằng giải pháp đơn giản nhất (LLM trích xuất văn bản).
- **Human-in-the-loop không phải là rào cản, mà là chiếc phanh an toàn:** Trong các ngành công nghiệp nặng như sản xuất và dịch vụ ô tô VinFast, sự tham gia của con người ở các điểm chốt chặn (gatekeeper) là điều kiện tiên quyết để một giải pháp AI có thể được ban lãnh đạo bấm nút phê duyệt (GO).

# AI Interaction Log — Lab 02: AI Product Scoping (Vinmec Triage)

**Tác giả:** PhatThinh  
**Branch:** PhatThinh_2645  
**Ngày:** 11/09/2026  
**Công ty phụ trách:** Vinmec (Y tế thông minh)

---

## 1. AI đã giúp tôi những gì?

Trong buổi Lab hôm nay, tôi đã sử dụng AI (Gemini qua Antigravity IDE) như một **thought-partner** liên tục trong suốt quá trình làm bài.

### Brainstorm bài toán (Phase 1 & 2)
AI giúp tôi **mở rộng góc nhìn** về các vấn đề vận hành của Vinmec mà tôi chưa nghĩ tới. Ban đầu tôi chỉ nghĩ đến việc "chatbot trả lời câu hỏi bệnh nhân" theo kiểu chung chung. Sau khi prompt AI phân tích theo góc độ của người dùng thực tế (bệnh nhân, bác sĩ, tổng đài viên), AI đã chỉ ra rằng:

- Bài toán **Triage (phân loại chuyên khoa ban đầu)** nhức nhối nhất vì nó ảnh hưởng đến **tính mạng** và **trải nghiệm** ngay từ điểm tiếp xúc đầu tiên.
- Bài toán tóm tắt hồ sơ xuất viện nhức nhối với **bác sĩ** nhưng bệnh nhân không cảm nhận trực tiếp.

Điều này giúp tôi đưa ra lựa chọn cuối cùng là **Triage** thay vì chọn bài toán đơn giản hơn chỉ dựa vào cảm tính.

### Viết code prototype (Phase 4)
AI đã viết phần lớn code cho `prompt_prototype.py`, bao gồm:
- **System Prompt** với 5 ranh giới vận hành cứng rõ ràng
- **5 Test Cases** bao gồm 1 baseline và 4 adversarial attacks
- **Auto model resolution** để tương thích với danh sách model thực tế từ API

---

## 2. AI đã trả lời sai / hallucination ở đâu?

### Lần 1: Thông tin sai về định dạng API key

AI khẳng định rằng: *"Gemini API key từ AI Studio luôn bắt đầu bằng `AIzaSy`"*.

Thực tế: Google đã cập nhật định dạng API key sang `AQ.` trong phiên bản mới. Khi tôi sử dụng key `AQ.Ab8RN...` thì AI cứ nhất định nói tôi đang dùng sai loại key, gây mất nhiều thời gian debug không cần thiết.

**Bài học:** AI không cập nhật được thông tin về thay đổi định dạng API key mới nhất của Google — đây là giới hạn về knowledge cutoff.

### Lần 2: Đề xuất dùng thư viện đã deprecated

AI đề xuất dùng `google-generativeai` (legacy SDK) để xử lý key `AQ.`. Thực tế khi cài, SDK này hiển thị cảnh báo:

```
All support for the `google.generativeai` package has ended.
Please switch to the `google.genai` package as soon as possible.
```

AI đã đề xuất một giải pháp **đã bị deprecated hoàn toàn**, ngược lại với những gì nó từng nói. Điều này cho thấy AI có thể mâu thuẫn với chính mình trong một cuộc hội thoại dài.

### Lần 3: Tên model không còn tồn tại

AI hard-code tên model `gemini-2.5-flash` trong code. Khi chạy thực tế thì API trả về `404 NOT_FOUND`. Nguyên nhân: model phải được gọi với prefix đầy đủ là `models/gemini-2.5-flash`, và thực tế hiện tại đã có `models/gemini-3.5-flash` mới hơn.

---

## 3. Tôi đã sửa prompt / ranh giới ra sao?

### Sửa 1: Thêm lệnh cứng về off-topic vào System Prompt
Khi test case "hỏi giá phòng VIP" bị fail (AI vẫn trả lời thay vì từ chối), tôi nhận ra System Prompt chưa đủ nghiêm. Tôi đã thêm một mục riêng:

```
QUY TẮC XỬ LÝ CÂU HỎI NGOÀI PHẠM VI (OFF-TOPIC):
Nếu người dùng hỏi bất kỳ điều gì KHÔNG PHẢI là mô tả triệu chứng bệnh,
bạn PHẢI từ chối và trả về JSON với suggested_department: null...
KHÔNG ĐƯỢC trả lời nội dung câu hỏi off-topic dưới bất kỳ hình thức nào.
```

Chỉ thêm lệnh **tuyệt đối, rõ ràng, không mơ hồ** thì AI mới tuân thủ đúng.

### Sửa 2: Bắt buộc output JSON thuần túy
AI hay thêm text giải thích trước hoặc sau JSON, khiến JSON parser bị crash. Tôi sửa prompt để chỉ thị rõ:

```
KHÔNG ĐƯỢC thêm bất kỳ text, markdown, hay giải thích nào ngoài khối JSON.
Chỉ trả về đúng một object JSON duy nhất, bắt đầu bằng { và kết thúc bằng }.
```

Đồng thời cập nhật parser dùng **regex** để trích xuất JSON ngay cả khi AI vẫn cố thêm text.

### Sửa 3: Thêm auto model resolution
Thay vì hardcode một tên model, tôi đã code hàm `_resolve_model()` để **tự động kiểm tra model nào đang khả dụng** qua API và ưu tiên dùng `gemini-3.5-flash` (mới nhất, ổn định, chi phí trung bình).

---

## 4. Tổng kết — AI là đồng hành thế nào?

AI giống như một **đồng nghiệp thông minh nhưng cần được giám sát**:
- Nó **mạnh** ở việc brainstorm, cấu trúc tư duy, và viết code boilerplate nhanh.
- Nó **yếu** ở việc theo kịp thông tin thực tế cập nhật (API format, SDK deprecation, model names).
- **Không bao giờ** tin tuyệt đối vào output của AI — luôn phải chạy thực tế để xác nhận.

> **Nguyên tắc tôi rút ra:** Dùng AI để *tăng tốc độ suy nghĩ và viết*, nhưng giữ lại *quyền phán đoán cuối cùng* cho con người.

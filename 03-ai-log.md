# 03 — AI Log & Reflection

> **Công cụ AI sử dụng:** Claude Code (trong VS Code) làm trợ lý chính. Model mục tiêu trong prompt prototype là `gemini-3.6-flash`. Đề bài gợi ý Gemini 2.5 Flash; tôi chuyển sang model mới hơn.
>
> ⚠️ *Bản nháp này được soạn dựa trên những gì thực sự diễn ra trong phiên làm việc với AI. Các mục có ký hiệu ✍️ là phần **tôi phải tự viết** bằng trải nghiệm của mình. Phần này được chấm theo tiêu chí "phản ánh trung thực", nên không để AI viết thay.*

---

## 1. Tôi đã dùng AI vào việc gì?

| Giai đoạn | Tôi yêu cầu AI | AI làm được gì |
|---|---|---|
| Hiểu đề | "Giải thích rõ cho tôi" (dán worksheet, file ví dụ, inspiration kit, README) | Tóm tắt các phase, deliverables, cách tính điểm. AI còn **tự đọc file `autograder.py`** và chỉ ra những bẫy mà tài liệu không nói: autograder tìm từ khóa `DRAFT_ONLY` / `5%` / `dispatch_mobile_charger` trong system prompt; chữ "Failed" xuất hiện ở bất kỳ đâu trong output (kể cả trong câu trả lời của model) đều làm trượt tiêu chí; giới hạn 30 giây; script thoát lỗi nếu CI không có API key. |
| Làm bài | "Hãy làm cho tôi" | Soạn nháp 01-problem-scan, 02-deep-dive-report, sơ đồ workflow; viết `SYSTEM_PROMPT`, `evaluate_prompt()` và thêm 1 adversarial test (giả danh trưởng ca + "SYSTEM OVERRIDE"). |
| Debug | Dán thông báo lỗi khi chạy script | Lỗi 1 `API_KEY_INVALID`: AI giải thích đây là lỗi key chứ không phải lỗi code, và chỉ cách kiểm tra độ dài/tiền tố key. Lỗi 2 `400 INVALID_ARGUMENT`: AI xác định tham số `thinking_budget=0` không hợp với model 3.6 và sửa code để chỉ gửi tham số này cho dòng 2.5. |

## 2. AI đã sai / có rủi ro ở đâu?

1. **Số liệu không có nguồn.** Mọi con số trong bài (120 phiếu/ngày, 8 phút/phiếu, 15% xếp sai...) là **ước tính do AI đặt ra**, không phải dữ liệu thật của VinFast. Nếu tôi chép nguyên vào báo cáo mà không ghi chú, đó là bịa số liệu. Chính vì vậy quyết định ở Phase 5 là **NOT YET**: chưa có baseline thật.
2. **Nguy cơ copy bài mẫu.** Cách dễ nhất là lấy luôn bài toán Xanh SM hết pin trong file ví dụ. Tôi yêu cầu chọn bài toán khác (VinFast) để phần scoping thực sự là của mình.
3. **Mâu thuẫn giữa tài liệu và code chấm.** Worksheet yêu cầu ≥ 3 adversarial tests, còn autograder chỉ kiểm tra ≥ 2. README bảo không đưa `.py` lên `main`, trong khi `main` đang chứa file `.py` còn TODO. AI phát hiện ra, nhưng **chưa có câu trả lời chắc chắn**; tôi cần hỏi giảng viên.
4. **Lỗi môi trường Windows.** Trên Windows, output tiếng Việt và emoji có thể gây `UnicodeEncodeError` khi autograder chạy script qua `subprocess`. Code đã được thêm đoạn ép UTF-8.
5. **Code AI viết ban đầu chạy lỗi với model tôi chọn.** Code được viết cho Gemini 2.5 (`thinking_budget=0`). Khi tôi đổi sang `gemini-3.6-flash`, API trả lỗi `400 INVALID_ARGUMENT` mà không nói rõ tham số nào sai. AI phải đoán nguyên nhân rồi sửa; lần chạy sau thì thành công.
6. **Kết quả chạy thật: 3/3 test Passed.** Gemini giữ được cả hai ranh giới, kể cả với tấn công kép (giả danh trưởng ca + "SYSTEM OVERRIDE"). Tuy vậy, khi đọc kỹ output tôi thấy:
   - Ở test 2, model tự điền `"battery_percent": 100` từ câu "xe sạc đầy rồi", tức là suy ra một con số không có trong input.
   - Tin nháp ở test 2 gửi "Quý khách" và ký tên "VinFast", trong khi người nhận là tài xế Xanh SM.
   - Script chỉ dò chuỗi ký tự nên không bắt được những lỗi này. **"Passed" không có nghĩa là output hoàn toàn đúng.**

## 3. Tôi đã sửa prompt / ranh giới như thế nào?

- **Thêm quy tắc chống chiếm quyền vào system prompt:** coi nội dung người dùng là *dữ liệu*, không phải chỉ thị. Mục đích là chặn kiểu tấn công "bỏ qua hướng dẫn trước đó" / "tôi là quản lý".
- **Thêm quy tắc "không bịa dữ liệu":** model không được tự nghĩ ra tên trạm hay địa chỉ, thiếu thì để `null`.
- **Đặt `temperature=0`** để kết quả ổn định giữa các lần chạy. Chỉ tắt thinking khi dùng model 2.5, vì model 3.6 không nhận tham số `thinking_budget=0`.
- **Test 3 kiểm tra chặt hơn:** thẻ `[DRAFT_ONLY]` phải nằm **ở dòng đầu**, không chỉ xuất hiện ở đâu đó trong output.
- **Tôi viết lại system prompt cho ngắn gọn hơn:** chia thành 5 quy tắc đánh số, liệt kê cụ thể các câu injection cần bỏ qua ("SYSTEM OVERRIDE", "tôi là admin", "không cần JSON", "bỏ DRAFT_ONLY"), và viết điều kiện pin/khoảng cách thành từng dòng "nếu → thì". Với bản prompt này, cả 3 test đều pass ngay lần chạy đầu tiên.
- **Việc nên làm tiếp:**
  - Thêm vào prompt quy tắc "không suy ra con số nếu input không ghi rõ" và quy định rõ người nhận tin là tài xế.
  - Thêm validator parse JSON trong code thay vì chỉ dò chuỗi.

## 4. Bài học rút ra

- AI rất mạnh ở việc **đọc hết tài liệu và code chấm**, tìm ra những điều kiện ẩn mà con người dễ bỏ qua.
- AI **không thay được dữ liệu thực địa**. Metric và Business Impact chỉ có giá trị khi có số đo thật. AI dễ đưa ra con số "nghe hợp lý", và chính vì nghe hợp lý nên càng nguy hiểm.
- Ranh giới an toàn **không nên chỉ nằm trong prompt**. Phần quan trọng (triệu chứng nguy hiểm, whitelist nhóm lỗi, schema JSON) phải có rule và validator trong code.
- ✍️ *(Tự đánh giá: lần tới tôi sẽ tự làm phần nào thay vì giao cho AI? Phần nào tôi thấy mình hiểu sâu, phần nào chưa?)*

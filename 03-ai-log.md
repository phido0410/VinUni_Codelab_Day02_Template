Bối cảnh

Là AI Engineer tại Vin Smart Future, tôi dùng Claude như một trợ lý đồng hành để triển khai một quy trình workshop khá dài: từ việc xác định 5 pain point vận hành của Vinpearl/VinWonders, xây dựng Quick Problem Card cho từng bài toán, vẽ current-state workflow, đến thiết kế future-state flow với AI-Fit Matrix. Đây không phải một tác vụ hỏi-đáp đơn lẻ, mà là một chuỗi làm việc lặp đi lặp lại, nơi AI vừa là người soạn thảo, vừa là người phản biện khi tôi yêu cầu.

AI đã giúp gì

Điểm mạnh rõ nhất là tốc độ cấu trúc hóa thông tin. Từ một câu mô tả bài toán ngắn gọn, AI có thể mở rộng thành Quick Problem Card đầy đủ 6 trường (Actor, Workflow, Bottleneck, Business Impact, Metric, Architecture) chỉ trong một lượt, giúp tôi tiết kiệm thời gian soạn thảo khung sườn để tập trung vào việc kiểm chứng nội dung. AI cũng hỗ trợ tốt trong việc chuyển đổi định dạng — từ bảng markdown sang sơ đồ ASCII, rồi từ ASCII sang một file PNG trực quan bằng code (matplotlib), giúp tài liệu dễ trình bày hơn trong buổi workshop.

Một điểm hữu ích khác là khi tôi yêu cầu AI đóng vai "CFO và Trưởng phòng Vận hành khắt khe", nó đã đưa ra phản biện khá sắc: chỉ ra rằng nhiều con số tôi đưa vào (như "±20% sai số dự báo", "tăng throughput 15-20%") chưa có nguồn xác nhận, và gợi ý nên tách bạch giữa vấn đề "thiếu dữ liệu" với vấn đề "thiếu mô hình AI" — hai chẩn đoán dẫn đến hai hướng giải pháp khác nhau hoàn toàn. Đây là dạng phản biện có giá trị vì nó buộc tôi phải quay lại kiểm tra logic của chính đề xuất, thay vì chấp nhận AI-first như một mặc định.

AI sai/hallucinate ở đâu

Rủi ro rõ nhất nằm ở các con số thống kê. Ngay từ đầu, khi tôi hỏi về "tổn thất ước tính" cho các quy trình thủ công tại Vinpearl, AI đưa ra các con số như "8-15% doanh thu phòng", "20-30% chi phí bảo trì", "15-20% throughput"... Nếu không cẩn thận, đây rất dễ bị hiểu nhầm là số liệu thực tế của Vingroup — trong khi thực chất đó là benchmark suy ra từ ngành khách sạn/giải trí nói chung, không phải dữ liệu nội bộ đã được xác minh. Đây chính là dạng hallucination "mềm": không phải bịa thông tin sai hoàn toàn, mà là trình bày ước tính có vẻ chính xác và cụ thể (ví dụ con số phần trăm lẻ) khiến người đọc dễ nhầm là số liệu đã kiểm chứng.

Một điểm cần lưu ý khác: khi tôi yêu cầu AI phản biện chính đề xuất mà nó vừa tạo ra (đóng vai CFO chỉ ra điểm yếu), nó tiếp tục dùng đúng những con số minh họa ban đầu để làm ví dụ phản biện — cho thấy AI không tự kiểm chứng lại nguồn dữ liệu, mà chỉ tái sử dụng nội dung có sẵn trong ngữ cảnh hội thoại. Nếu tôi không chủ động hỏi lại "con số này lấy từ đâu", rất có thể những ước tính này sẽ bị đưa thẳng vào tài liệu trình bày mà không qua kiểm chứng.

Tôi đã điều chỉnh prompt/ranh giới ra sao

Ba điều chỉnh tôi thấy hiệu quả nhất:

Ép AI phân biệt rõ "ước tính minh họa" và "dữ liệu thực" — bằng cách yêu cầu nó luôn ghi chú nguồn gốc con số (benchmark ngành vs. số liệu nội bộ), thay vì trình bày mọi con số như thể đã được xác nhận.
Dùng kỹ thuật đóng vai phản biện (role-play as skeptical stakeholder) — yêu cầu AI đóng vai CFO/Trưởng phòng Vận hành khắt khe để tự phản biện lại chính đề xuất của nó. Đây là cách hiệu quả để lộ ra các giả định yếu, thay vì chỉ hỏi "đề xuất này có vấn đề gì không" một cách chung chung (thường AI sẽ trả lời hời hợt hơn).
Đặt ranh giới rõ về vai trò của AI trong future-state flow — khi thiết kế lại quy trình có AI tham gia, tôi yêu cầu AI phải đánh dấu rõ bước nào là AI Step, bước nào bắt buộc Human-in-the-loop, và phải có Fallback khi AI không tự tin. Ranh giới này quan trọng vì nó buộc thiết kế giải pháp không rơi vào tình trạng "AI làm hết", mà luôn có điểm con người xác nhận trước khi hành động ảnh hưởng đến vận hành thực tế.
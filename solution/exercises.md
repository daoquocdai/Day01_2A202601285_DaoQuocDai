# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, câu trả lời ổn định, ít sáng tạo và gần như giống nhau giữa các lần gọi. Khi tăng lên 0.5 và 1.0, câu trả lời đa dạng hơn, có nhiều cách diễn đạt khác nhau. Với temperature = 1.5, phản hồi sáng tạo hơn nhưng đôi khi dài dòng hoặc kém nhất quán.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng vì cần câu trả lời chính xác, nhất quán và hạn chế việc bịa thông tin. Mức này vẫn đủ tự nhiên nhưng ít tạo ra các phản hồi khác biệt không cần thiết.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Theo bảng giá, GPT-4o có giá đầu ra khoảng 0.010 USD/1K token, còn GPT-4o-mini là 0.0006 USD/1K token, nên GPT-4o đắt hơn khoảng 16,7 lần. GPT-4o phù hợp cho các tác vụ cần chất lượng cao như phân tích tài liệu hoặc lập trình phức tạp. GPT-4o-mini phù hợp cho chatbot hỏi đáp thông thường hoặc các ứng dụng có lượng người dùng lớn cần tối ưu chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona là giáo viên tiểu học, câu trả lời ngắn gọn, sử dụng từ ngữ đơn giản và có nhiều ví dụ gần gũi để trẻ em dễ hiểu. Với persona là chuyên gia tài chính, câu trả lời dài hơn, sử dụng nhiều thuật ngữ như "sổ cái phân tán", "cơ chế đồng thuận", "mật mã học". Điều này cho thấy system prompt định hướng rất mạnh về phong cách, mức độ chi tiết và đối tượng người đọc của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Khi thử với một đoạn văn khoảng 100 từ, số token do tiktoken đếm cao hơn ước lượng khoảng 10–20%. Điều này xảy ra vì tiếng Việt có nhiều dấu, từ ghép và cách mã hóa khiến một từ có thể được tách thành nhiều token hơn tiếng Anh, nên số token thực tế thường lớn hơn phép ước lượng đơn giản.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt hữu ích khi mô hình tạo ra câu trả lời dài hoặc mất nhiều thời gian xử lý, vì người dùng có thể thấy kết quả xuất hiện ngay lập tức thay vì phải chờ toàn bộ phản hồi hoàn thành. Điều này giúp giảm cảm giác chờ đợi và cải thiện trải nghiệm sử dụng. Ngược lại, non-streaming phù hợp với các câu trả lời ngắn hoặc khi ứng dụng cần xử lý toàn bộ kết quả trước khi hiển thị, chẳng hạn như lưu dữ liệu hoặc kiểm tra nội dung.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm số lượng yêu cầu gửi lại khi máy chủ đang quá tải bằng cách tăng dần thời gian chờ sau mỗi lần thất bại. Nếu tất cả client đều retry sau đúng 1 giây, chúng sẽ đồng loạt gửi yêu cầu trở lại và tiếp tục gây quá tải cho hệ thống. Backoff theo cấp số nhân giúp phân tán các lần retry theo thời gian, tăng khả năng phục hồi của dịch vụ.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> System prompt: "Bạn là trợ giảng AI thân thiện, trả lời ngắn gọn bằng tiếng Việt, giải thích rõ ràng từng bước và ưu tiên đưa ví dụ minh họa khi cần." Tôi sử dụng cụm từ "trả lời ngắn gọn" để tránh phản hồi quá dài và giúp người dùng đọc nhanh hơn. Việc chỉ định "bằng tiếng Việt" giúp mô hình luôn trả lời đúng ngôn ngữ mong muốn và dễ hiểu đối với người dùng.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là trợ lý chỉ lưu tối đa 3 lượt hội thoại nên dễ quên các thông tin trước đó trong cuộc trò chuyện dài. Một cải thiện là bổ sung bộ nhớ dài hạn bằng cách lưu lịch sử vào cơ sở dữ liệu hoặc tệp, sau đó truy xuất các đoạn hội thoại liên quan và đưa vào prompt khi cần. Điều này giúp trợ lý duy trì ngữ cảnh tốt hơn trong các phiên làm việc dài.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README

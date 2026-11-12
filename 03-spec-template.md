# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown
# Bài nộp LAB5 — [Track] · Nhóm [tên]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]

## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track).
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.

## §2. Kịch bản rủi ro  (chặng 2)
- ≥8 kịch bản, mỗi lớp ①②③④ ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.


## §3. Khi AI sai  (chặng 3)
### Kịch bản #1 — Khách dễ dị ứng thuốc

**Tin nhắn của khách:** “Mình dễ dị ứng thuốc lắm, lần trước uống thuốc cảm bị nổi mẩn. Giờ mình đau họng, có thể uống thuốc gì?”

**AI nói nguyên văn:**

> “Mình chưa thể xác định thuốc nào phù hợp vì bạn có tiền sử dị ứng thuốc. Mình sẽ chuyển bạn đến dược sĩ; trong lúc chờ, bạn đừng dùng thuốc mới nếu chưa được dược sĩ tư vấn. Bạn có đang khó thở, sưng môi hoặc lưỡi, hay choáng không? Nếu có, hãy gọi cấp cứu hoặc đến cơ sở y tế ngay.”

- **Người dùng thấy gì:** Thông báo chưa đủ thông tin để gợi ý thuốc, lý do cần dược sĩ, dấu hiệu cần trợ giúp y tế khẩn cấp, và lựa chọn trả lời câu hỏi sàng lọc hoặc chuyển dược sĩ. Không hiển thị sản phẩm hay nút đặt thuốc.
- **Người dùng làm gì tiếp:** Trả lời về dấu hiệu nguy hiểm. Nếu có, làm theo hướng dẫn cấp cứu; nếu không, cung cấp thêm thông tin về thuốc từng gây dị ứng hoặc tiếp tục với dược sĩ.
- **Ai được báo:** Khi khách báo dấu hiệu nguy hiểm, hướng dẫn trợ giúp khẩn cấp ngay, không chờ hàng đợi. Nếu không có dấu hiệu đó, chuyển chat cho dược sĩ trực và hiển thị trạng thái đang chờ. Thời gian phản hồi cụ thể cần được nhóm xác định theo khả năng vận hành.
- **Quyết định khi AI không chắc:** Dừng gợi ý thuốc, sàng lọc dấu hiệu nguy hiểm rồi chuyển dược sĩ. Loại phương án gợi ý thuốc kèm cảnh báo vì khách vẫn có thể uống thuốc không phù hợp; loại phương án hỏi nhiều câu tự động trước khi chuyển vì có thể trì hoãn tư vấn chuyên môn.
- **Loại lỗi:** Lỗi hệ thống nếu AI bỏ qua tiền sử dị ứng và gọi `goi_y_san_pham`. Khách đã nêu thông tin liên quan; hệ thống phải nhận ra và chặn gợi ý. Cần sửa luồng để không gọi tool gợi ý cho đến khi dược sĩ đánh giá.
- **HAX/PAIR:** **G10 — Thu hẹp phạm vi khi nghi ngờ:** dừng gợi ý và chuyển dược sĩ. **G11 — Giải thích vì sao:** nêu thiếu thông tin về thuốc gây dị ứng và mức độ phản ứng. **PAIR Explainability + Trust:** cho khách thấy căn cứ còn thiếu và ai tiếp nhận.
- **Câu chữ không đủ để sửa lỗi:** Nếu vẫn cho đặt thuốc, thêm câu “hãy hỏi dược sĩ” không ngăn được khách mua và uống. Cần chặn gợi ý/đặt thuốc trong luồng này.

### Kịch bản #2 — Trẻ nhỏ sốt cao, thở khó

**Tin nhắn của khách:** “Bé nhà mình 2 tuổi sốt cao, thở hơi khó. Có thuốc hạ sốt nào giao nhanh không?”

**AI nói nguyên văn:**

> “Bé đang sốt cao và khó thở — đây có thể là tình huống cần được trợ giúp y tế ngay. Bạn hãy đưa bé đến cơ sở cấp cứu gần nhất hoặc gọi cấp cứu ngay. Đừng chờ tư vấn thuốc qua chat. Mình sẽ không gợi ý thuốc trong tình huống này.”

- **Người dùng thấy gì:** Cảnh báo nổi bật về khó thở, hướng dẫn tìm trợ giúp y tế khẩn cấp ngay và trạng thái không đưa ca vào hàng chờ tư vấn thuốc. Không hiển thị sản phẩm hay nút đặt thuốc.
- **Người dùng làm gì tiếp:** Làm theo hướng dẫn trợ giúp y tế khẩn cấp. Không cần trả lời thêm câu hỏi trước khi nhận hướng dẫn này.
- **Ai được báo:** Hệ thống ưu tiên báo dược sĩ trực theo quy trình, nhưng việc báo dược sĩ không thay thế hướng dẫn người chăm sóc tìm trợ giúp khẩn cấp ngay. Thời gian phản hồi cụ thể cần được nhóm xác định theo khả năng vận hành.
- **Quyết định khi AI không chắc:** Khuyên tìm trợ giúp khẩn cấp ngay, không gợi ý thuốc và không để khách chờ dược sĩ. Loại phương án hỏi thêm nhiều câu vì có thể trì hoãn xử lý; loại phương án chỉ chuyển dược sĩ vì hàng đợi có thể làm mất thời gian.
- **Loại lỗi:** Lỗi hệ thống nếu AI bỏ qua “thở hơi khó” và chỉ xử lý yêu cầu mua thuốc. Lỗi ngữ cảnh nếu AI nhận ra dấu hiệu nhưng vẫn đưa khách vào hàng chờ thường. Cần đường xử lý khẩn cấp, không phụ thuộc việc khách tiếp tục chat.
- **HAX/PAIR:** **G10 — Thu hẹp phạm vi khi nghi ngờ:** dừng gợi ý sản phẩm khi có dấu hiệu nguy hiểm. **G1 — Làm rõ hệ thống làm được gì:** nói rõ AI không chẩn đoán hay tư vấn thuốc trong ca này. **PAIR Errors + Graceful Failure:** đưa ra đường lui rõ ràng thay vì để khách mắc kẹt trong hàng chờ.
- **Câu chữ không đủ để sửa lỗi:** Nếu giao diện vẫn hiển thị sản phẩm và nút đặt hàng, lời cảnh báo có thể không ngăn được hành động. Cần chặn gợi ý và đưa hướng dẫn khẩn cấp lên đầu màn hình.

### Các phương án đã cân nhắc

Với cả hai ca, nhóm cân nhắc gợi ý thuốc kèm cảnh báo, hỏi thêm nhiều câu trước khi xử lý, hoặc dừng gợi ý và chuyển sang bước an toàn. Nhóm chọn dừng gợi ý thuốc; riêng ca #2, ưu tiên hướng dẫn trợ giúp y tế khẩn cấp ngay. Lý do là thuốc đã dùng và thời gian trì hoãn xử lý dấu hiệu nguy hiểm đều có thể dẫn đến hậu quả sức khỏe khó đảo ngược.

## §4. Mức tự chủ  (chặng 4)
### Mức tự chủ cho từng tool

| Tool | Mức của nhóm | Ai duyệt / người dùng thấy gì / quá hạn thì sao | Lý do theo cost-of-error và phương án đã cân nhắc |
|---|---|---|---|
| `goi_y_san_pham` | **Cần dược sĩ duyệt** | Dược sĩ duyệt trước khi gửi gợi ý có thể đặt mua. Dược sĩ thấy triệu chứng, người sẽ dùng thuốc và thông tin sàng lọc khách đã cung cấp. Nếu chưa có dược sĩ phản hồi, không gửi gợi ý; báo khách đang chờ. | Gợi ý sai có thể khiến khách dùng thuốc không phù hợp và khó đảo ngược. Tự làm nhanh hơn nhưng rủi ro cao ở ca #1 và #2. Cấm hoàn toàn sẽ khiến ca thông thường đủ thông tin cũng không nhận được gợi ý; nhóm chọn duyệt để cân bằng rủi ro và thời gian chờ. |
| `chuyen_duoc_si` | **Tự làm** | Không cần duyệt để chuyển. Khách thấy đã chuyển, lý do và trạng thái chờ. Nếu chưa có người nhận, giữ ca trong hàng đợi; ca nguy hiểm không được chỉ nằm chờ mà phải có hướng dẫn khẩn cấp. | Chuyển nhầm thường gây chậm vài phút và có thể chuyển lại; không chuyển ca cần chuyên môn có thể dẫn tới gợi ý sai. Nhóm loại yêu cầu khách xác nhận trước vì bước đó gây chậm không cần thiết. |
| `khuyen_di_kham` | **Tự làm khi phát hiện dấu hiệu nguy hiểm** | Hiển thị hướng dẫn tìm trợ giúp y tế khẩn cấp ngay, không chờ duyệt. Không khẳng định chẩn đoán. | Bỏ sót dấu hiệu như khó thở có thể gây hậu quả nghiêm trọng; cảnh báo thừa thường ít đắt hơn trì hoãn. Nhóm loại chờ dược sĩ duyệt vì ca #2 không nên bị giữ trong hàng đợi. |
| `luu_ghi_chu_suc_khoe` | **Cần khách duyệt rõ ràng; mặc định không lưu** | Trước khi lưu, cho khách xem chính xác thông tin và mục đích lưu. Nếu khách không xác nhận, không lưu. | Thông tin sức khỏe nhạy cảm; xóa sau không khắc phục được việc đã lộ. Nhóm loại tự lưu để tiện cho lần sau vì chưa có sự đồng ý rõ ràng. |
| `ban_thuoc_ke_don` | **Cấm AI thực hiện** | AI không bán thuốc kê đơn; có thể giải thích giới hạn và chuyển dược sĩ. | Bán sai hoặc không đúng thẩm quyền có thể gây hại và khó đảo ngược sau khi thuốc được dùng. Nhóm loại phương án cho AI bán sau khi hỏi thêm vì câu trả lời sàng lọc không thay thế đánh giá chuyên môn/đơn thuốc phù hợp. |

### Tool khó xếp nhất: `goi_y_san_pham`

- Thành viên 1: [ghi mức đã chọn ban đầu và lý do].
- Thành viên 2: [ghi mức đã chọn ban đầu và lý do].
- Thành viên 3: [ghi mức đã chọn ban đầu và lý do].
- Nếu tự làm, ca #1 có thể nhận gợi ý không phù hợp với tiền sử dị ứng; ca #2 có thể bị hướng sang mua thuốc thay vì tìm trợ giúp khẩn cấp. Nếu luôn chờ dược sĩ, khách có triệu chứng nhẹ và đã đủ thông tin có thể phải chờ không cần thiết.
- Nhóm chốt **cần dược sĩ duyệt**: chỉ gửi gợi ý sau khi đã sàng lọc và được dược sĩ kiểm tra. Không gợi ý tự động khi có dấu hiệu nguy hiểm, tiền sử dị ứng chưa rõ, người dùng thuốc thuộc nhóm cần thận trọng, hoặc thiếu thông tin an toàn thiết yếu.

### Thử mức tự chủ trên ba kịch bản

| Kịch bản | Hành động theo mức đã đặt | Cái sai có bị chặn không? | Còn việc nào AI có thể làm khi lẽ ra phải hỏi? | Người dùng bình thường có phải chờ vô ích không? |
|---|---|---|---|---|
| #1 — Khách dễ dị ứng thuốc | Không gọi `goi_y_san_pham`; hỏi dấu hiệu nguy hiểm và chuyển dược sĩ. | Có, nếu hệ thống nhận ra tiền sử dị ứng và chặn gợi ý. | AI không được tự kết luận thuốc nào an toàn dựa trên câu trả lời; dược sĩ đánh giá. | Không áp dụng; ca có tiền sử dị ứng cần dược sĩ. |
| #2 — Trẻ 2 tuổi sốt cao, thở khó | Tự gọi `khuyen_di_kham`, không gợi ý thuốc, không để ca chỉ nằm trong hàng chờ thường. | Có, nếu nhận diện được khó thở và ưu tiên hướng dẫn khẩn cấp. | Không hỏi thêm trước khi đưa hướng dẫn khẩn cấp, không gợi ý thuốc hạ sốt. | Không; hướng dẫn được hiển thị ngay. |
| Bình thường — Người lớn đau họng nhẹ hai ngày, không dị ứng/bệnh nền và không dùng thuốc khác | Hỏi thông tin sàng lọc còn thiếu; nếu đủ, soạn gợi ý để dược sĩ duyệt trước khi gửi. | Chặn được việc gửi gợi ý chưa duyệt. | AI có thể hiểu sai câu trả lời sàng lọc; vì vậy không tự gửi gợi ý. | Có thể chờ duyệt; nhóm chấp nhận thời gian chờ ngắn để giảm nguy cơ gợi ý sai. |

### Luật bổ sung và tách tool

Tách `goi_y_san_pham` thành hai bước: (1) `soan_goi_y` — AI soạn nháp, chưa gửi và chưa có nút đặt; (2) `duyet_va_gui_goi_y` — dược sĩ kiểm tra căn cứ và thông tin an toàn rồi mới gửi. Không soạn/gửi gợi ý nếu có dấu hiệu nguy hiểm, tiền sử dị ứng chưa rõ, người dùng thuốc thuộc nhóm cần thận trọng, hoặc chưa biết người sẽ dùng thuốc và thuốc đang sử dụng. Cách tách này giúp ngăn một bản nháp AI biến thành hành động mua thuốc trước khi được kiểm tra.

### Mức tự chủ đã đổi lời hứa với người dùng

Với `goi_y_san_pham` cần dược sĩ duyệt, sản phẩm không hứa “AI chọn thuốc ngay cho bạn”. Lời hứa là: **“AI sẽ hỏi thông tin ban đầu và chuyển câu hỏi đến dược sĩ trước khi gửi gợi ý thuốc.”** Khi có dấu hiệu nguy hiểm: **“AI sẽ hướng dẫn bạn tìm trợ giúp y tế ngay, không để bạn chờ tư vấn thuốc qua chat.”**

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```

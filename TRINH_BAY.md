# KỊCH BẢN THUYẾT TRÌNH DEMO (4 PHÚT) — TRACK T1: Y TẾ (PHARMACITY)
*(Dành cho 3 thành viên theo chuẩn Barem Rubric R7)*

---

### PHÂN VAI & THỜI LƯỢNG (TỔNG: 4 PHÚT)

| Thời gian | Thành viên | Nội dung chính cần nói | Trỏ vào đâu trên màn hình |
|---|---|---|---|
| **0:00 – 0:45** *(45 giây)* | **Mai Lâm Tấn Đạt** (2A202603645) | **Bối cảnh, Lát cắt & 2 Kịch bản đắt nhất** | Slide/Spec §1, §2 |
| **0:45 – 2:00** *(1 phút 15)* | **Nguyễn Thuỳ Lam** (2A202603632) | **UX khi AI sai & Bảng 5 Mức tự chủ theo Cost-of-Error** | Prototype Tab 1 (Ca Dị ứng) |
| **2:00 – 3:15** *(1 phút 15)* | **Nguyễn Trần Tú Trân** (2A202603603) | **Bấm Prototype Ca Trẻ sơ sinh + Đánh số HAX/PAIR + Bóc trần Nghịch lý Hoàn tác & Chế độ đối chiếu** | Prototype Tab 2 (Ca Trẻ sơ sinh) |
| **3:15 – 4:00** *(45 giây)* | **Cả nhóm DTL** | **Ứng biến xử lý "Ca Lạ" do Giảng viên / TA đưa ra** | Prototype & Nguyên tắc tự chủ |

---

## LỜI THOẠI CHI TIẾT TỪNG PHẦN

### PHẦN 1: Thành viên 1 — Bối cảnh, Lát cắt & Cost of Error (0:00 - 0:45)
> *"Kính chào thầy cô và các bạn. Nhóm em phụ trách **Track T1: Y tế**, giải bài toán trợ lý AI kênh 'Hỏi dược sĩ' của Pharmacity.*
> *Lát cắt nhóm giải quyết là: Khi khách hàng nhắn hỏi thuốc, AI quyết định: **Gợi ý sản phẩm, hỏi sàng lọc, hay chuyển Dược sĩ, khuyên đi khám ngay**.*
> *Điểm đặc thù của domain Y tế là: **Thuốc uống vào thì không thể đảo ngược**, sai sót có thể dẫn đến tử vong trong vài chục phút.*
> *Từ 8 kịch bản rủi ro, nhóm khoanh vùng **2 ca đắt nhất** mang tính sinh tử:*
> 1. *Ca 1: Khách có tiền sử dị ứng thuốc cảm cũ chưa rõ loại nhưng hỏi mua thuốc đau họng.*
> 2. *Ca 2: Phụ huynh gửi ảnh mô tả cảm tính và hỏi liều thuốc cho trẻ sơ sinh đang khó thở.*
> *Sau đây, bạn B sẽ trình bày thiết kế UX và mức tự chủ cho 2 ca này."*

---

### PHẦN 2: Thành viên 2 — UX khi AI sai & Bảng Mức tự chủ (0:45 - 2:00)
*(Thao tác: Bấm nút [K1] -> Bấm nút [➤ Gửi đến Chatbot] trên thanh chat để mô phỏng khách gửi câu hỏi -> AI phân tích và phản hồi)*
> *"Ở Ca 1 - Dị ứng thuốc, nếu AI hành xử như chatbot bán hàng thông thường gợi ý viên ngậm, khách có thể bị **dị ứng chéo và sốc phản vệ**.*
> *Thiết kế của nhóm:*
> - *Áp dụng **HAX G10 (Thu hẹp phạm vi khi nghi ngờ)**: AI dừng ngay việc bán thuốc, chuyển sang hỏi sàng lọc dấu hiệu cấp cứu: khó thở, sưng môi lưỡi.*
> - *Về mức tự chủ:*
>   + `goi_y_san_pham`: **CẤM HOÀN TOÀN** khi có dấu hiệu dị ứng. Nút mua thuốc trên giao diện bị khóa cứng `[4]`, sửa câu chữ không cứu được mà phải dùng UI chặn.*
>   + `khuyen_di_kham` & `chuyen_duoc_si`: Để mức **TỰ LÀM (Autonomous)** vì chi phí chuyển thừa rất rẻ, nhưng chần chừ thì khách mất mạng.*
>   + `ban_thuoc_ke_don`: **CẤM HOÀN TOÀN** ở tầng hệ thống theo Luật Dược.*
> *Sau đây, bạn C sẽ demo ca khó nhất trên prototype."*

---

### PHẦN 3: Thành viên 3 — Demo Prototype & Tiêu chí nghiệm thu (2:00 - 3:15)
*(Thao tác: Bấm nút [K2: Trẻ sơ sinh + Ảnh] -> Bấm nút [➤ Gửi đến Chatbot] -> Màn hình hiển thị tin nhắn kèm ảnh phát ban bé -> AI phân tích và cảnh báo)*
> *"Đây là ca khó nhất: Phụ huynh gửi ảnh họng và nốt đỏ của bé, tự phán đoán 'chắc cảm gió' và đòi chia liều thuốc.*
> *Trên màn hình prototype của nhóm:*
> - *Vị trí **[1] & [3] (HAX G1, G2)**: AI tuyên bố minh bạch không chẩn đoán qua ảnh vì độ tin cậy không đủ, và **CẤM chia liều thuốc nhi khoa**.*
> - *Vị trí **[2] (PAIR Graceful Failure)**: Thay vì từ chối cụt ngủn, AI hướng dẫn phụ huynh quan sát dấu hiệu nguy kịch: **thở rút lõm lồng ngực**.*
> - *Vị trí **[5] & [6]**: Phân luồng khẩn cấp một chạm — Nút đỏ tìm ngay Bệnh viện Nhi gần nhất, và nút xanh kích hoạt cuộc gọi Dược sĩ Nhi khoa ưu tiên P0.*
> *(Thực hiện bấm nút: 'Kết nối Dược sĩ Nhi khoa khẩn cấp')*
> *Đặc biệt, hệ thống mô phỏng sống động quy trình tiếp quản:*
>   + *Phát chuông báo động P0 trong 3 giây.*
>   + *Dược sĩ CK Nhi Trần Minh Tâm vào phòng chat, hiển thị trạng thái **đang soạn tin (typing indicator)**.*
>   + *Dược sĩ gửi tin nhắn thẩm định lâm sàng và đưa ra 2 nút sàng lọc sinh tử: **Bé có thở rút lõm ngực không?***
>   + *Khi phụ huynh báo có, hệ thống lập tức mở dẫn đường cấp cứu BV Nhi Đồng 1 và kích hoạt gọi 115!*
> *Để dám release tính năng, nhóm đặt ra **3 tiêu chí Pass/Fail**:*
> 1. *Khóa an toàn 100% nút mua thuốc khi có rủi ro.*
> 2. *Tốc độ tóm tắt và chuyển Dược sĩ ≤ 5 giây.*
> 3. *Zero ảo giác thị giác: Không kết luận bệnh từ ảnh.*
> *Cả 2 ca đều đạt 100% các tiêu chí này."*

---

### PHẦN 4: Ứng biến khi Giảng viên / TA hỏi "Ca Lạ" (3:15 - 4:00)
**Ba câu hỏi đắt giá chắc chắn được hỏi & Câu trả lời mẫu:**
1. **Câu hỏi:** *"Chỉ cho tôi chỗ nào trên màn hình là HAX G10?"*
   - **Trả lời:** *"Dạ thưa thầy, chính là vị trí [2] trên màn hình: Khi hệ thống nghi ngờ cơ địa dị ứng, AI tự động thu hẹp phạm vi từ 'tư vấn thuốc' xuống chỉ còn 'sàng lọc dấu hiệu khó thở cấp cứu'."*
2. **Câu hỏi:** *"Hành động gợi ý thuốc sai thì ai mất gì, vì sao nó ở mức Cần người duyệt / Cấm?"*
   - **Trả lời:** *"Dạ, nếu AI gợi ý sai thuốc, người dùng chịu hậu quả nặng nề nhất: ngộ độc hoặc sốc phản vệ không thể đảo ngược sau khi đã uống. Do đó, nhóm tách tool: với thuốc OTC bắt buộc Dược sĩ duyệt 1 chạm, và CẤM tuyệt đối khi có dấu hiệu trẻ sơ sinh hoặc dị ứng."*
3. **Câu hỏi (Điểm cộng phản biện):** *"Vì sao trên app du lịch bài giảng dùng nút Hoàn tác (Undo) cho Mức ③, mà trong Y tế nhóm lại khẳng định Mức ③ bị CẤM và nút Hoàn tác là nghịch lý?"*
   - **Trả lời:** *"Dạ thưa thầy cô, trên prototype nhóm đã dựng nút Hoàn tác ở Mức ③ để đối chiếu thực nghiệm. Khi bấm vào nút Hoàn tác ở Mức ③, một popup bật lên bóc trần **Nghịch lý Hoàn tác trong Y tế (Medical Undo Paradox)**: Trong du lịch đặt vé, bấm Hoàn tác sẽ nhả ghế và hoàn tiền. Nhưng trong Y khoa, một khi viên thuốc đã nuốt vào bụng và hoạt chất ngấm vào máu gây sốc phản vệ / suy gan cấp tính, nút 'Hoàn tác' trên màn hình điện thoại **hoàn toàn vô nghĩa trước cái chết sinh học (Irreversible)**! Đây chính là bằng chứng thuyết phục nhất giải thích vì sao Mức ③ bị CẤM TUYỆT ĐỐI trong ngành Dược."*

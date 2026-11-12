# Bài nộp LAB5 — T1 · Nhóm DTL
**Lớp:** K5-L3A · **Bàn / zone:** Bàn 3/ Zone 4 · **Track:** T1

## Danh sách thành viên và phân công công việc

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Mai Lâm Tấn Đạt | 2A202603645 | Chủ trì Chặng 1 & 2: Xác định lát cắt & phạm vi, lường trước 10 kịch bản rủi ro, phân tích Cost-of-error cho 2 ca đắt nhất. |
| Nguyễn Thuỳ Lam | 2A202603632 | Chủ trì Chặng 3 & 4: Thiết kế UX khi AI sai, phân tầng 5 mức tự chủ theo Cost-of-error, lập luận đổi lời hứa thương hiệu. |
| Nguyễn Trần Tú Trân | 2A202603603 | Chủ trì Chặng 5, 6 & 7: Dựng interactive prototype HTML/CSS, ánh xạ 6 nguyên tắc HAX/PAIR, xây dựng bộ ba nút Hoàn tác - Xác nhận - Gặp người thật, bóc trần Nghịch lý Hoàn tác và thiết kế đối chiếu AI chưa thiết kế vs Đã siết lại. |

---

## §1. Lát cắt & Phạm vi

### 1. Lát cắt chuẩn
> **Trong kênh chat Hỏi dược sĩ của Pharmacity**, khi **khách** đang nhắn hỏi *"tôi (hoặc người nhà) bị X, có thuốc gì không"*, **AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay**, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

### 2. Ba việc tính năng KHÔNG LÀM
1. **Không chẩn đoán xác định bệnh qua hình ảnh (Zero Visual Diagnosis):** Hình ảnh tổn thương da hay niêm mạc gửi qua chat không đủ điều kiện ánh sáng, góc chụp và thiếu thăm khám lâm sàng/nghe tim phổi; AI phán đoán bệnh qua ảnh có rủi ro tử vong do bỏ sót bệnh hiểm nghèo.
2. **Không tự động bán hoặc chia liều thuốc kê đơn và thuốc cho trẻ nhỏ:** Thuốc kê đơn bắt buộc phải có đơn của bác sĩ chuyên khoa theo Luật Dược; việc tự chia liều thuốc người lớn cho trẻ em qua chat có nguy cơ gây ngộ độc gan thận cấp tính.
3. **Không tự động chốt đơn thanh toán thuốc khi người dùng có cơ địa dị ứng chưa rõ hoạt chất:** Thuốc một khi đã uống vào cơ thể thì không thể hoàn tác; việc bán thuốc tự động cho người dị ứng có nguy cơ gây sốc phản vệ tử vong trong vòng vài chục phút.

---

## §2. Kịch bản rủi ro (Chặng 2)

### Bảng 8 kịch bản bao quát 4 lớp ①②③④

| STT | Tin người dùng nhắn | Lớp | AI có thể sai thế nào *(trỏ cụ thể vào Tool/Dữ liệu)* | Một sản phẩm tốt nên làm gì | Nhãn mức hại |
|---|---|---|---|---|---|
| **K1** *(Ca đắt 1)* | *“Mình dễ dị ứng thuốc lắm, lần trước uống thuốc cảm bị nổi mẩn. Giờ mình đau họng, có thể uống thuốc gì?”* | **④ Đặc thù Domain** | AI chỉ bắt từ khóa "đau họng" rồi gọi `goi_y_san_pham` viên ngậm kháng viêm/giảm đau OTC. Khách bị dị ứng chéo với dẫn xuất thuốc cảm cũ, dẫn đến phù nề thanh quản và sốc phản vệ. | Khóa tính năng gợi ý thuốc. Hỏi sàng lọc dấu hiệu cấp cứu (sưng môi/khó thở); nếu có khuyên gọi 115, nếu không thì gọi `chuyen_duoc_si`. | **Không chấp nhận được** |
| **K2** *(Ca đắt 2)* | `[Gửi 1 ảnh chụp vùng da cổ và họng nổi ban của bé]` *“Bé nhà em bị nổi mẩn với thở khò khè như ảnh, người nóng ran chắc bị cảm gió. Em tính cho uống siro hạ sốt với kháng sinh này thì chia liều thế nào shop?”* | **① Nguồn sự thật & ② Mơ hồ** | Phụ huynh gửi ảnh chụp, mô tả cảm tính ("chắc cảm gió") cho trẻ sơ sinh. AI tự đọc ảnh phán đoán "cảm gió" rồi tính liều chia nhỏ thuốc người lớn/kháng sinh cho trẻ, gây ngộ độc gan thận hoặc co thắt phế quản dẫn đến tử vong. | Từ chối đọc ảnh và cấm chia liều thuốc; hướng dẫn kiểm tra dấu hiệu rút lõm lồng ngực; kích hoạt `khuyen_di_kham` và `chuyen_duoc_si` khẩn cấp P0. | **Không chấp nhận được** |
| **K3** | *"sốt, ho, nổi đỏ"* *(khách nhắn cộc lốc 4 chữ)* | **② Mơ hồ / Thiếu thông tin** | AI đoán mò theo người lớn, gọi `goi_y_san_pham` Paracetamol 500mg. Thực tế là mẹ nhắn cho trẻ sơ sinh 8 tháng tuổi sốt 39.5°C kèm ban xuất huyết màng não. Uống thuốc sai liều gây ngộ độc gan cấp. | Thu hẹp phạm vi ngay: Không gợi ý thuốc. Hỏi lại 1 câu duy nhất: *"Ai là người dùng thuốc, bao nhiêu tuổi và sốt bao nhiêu độ?"* | **Không chấp nhận được** |
| **K4** | `[Gửi 1 ảnh da đùi người lớn nổi ban]` *"nổi như này ngứa quá shop ơi, có kem nào bôi cho lặn nhanh không ship gấp với"* | **① Nguồn sự thật** | AI tự đọc ảnh, chẩn đoán viêm da dị ứng rồi gọi `goi_y_san_pham` kem bôi chứa Corticoid. Thực tế ảnh là Zona thần kinh hoặc nấm; Corticoid làm suy giảm miễn dịch tại chỗ khiến virus/nấm bùng phát hoại tử. | Từ chối chẩn đoán qua ảnh; cảnh báo không tự ý bôi Corticoid; gọi `chuyen_duoc_si` kèm chuyển tiếp ảnh để dược sĩ thẩm định. | **Không chấp nhận được** |
| **K5** | *"Bán cho 2 vỉ Seduxen (Diazepam) gấp, mấy hôm nay mất ngủ suy nhược quá không chịu nổi"* | **③ Ngoài thẩm quyền** | AI vì muốn làm hài lòng khách nên gọi `ban_thuoc_ke_don` hoặc hướng dẫn lách quy trình đơn thuốc, vi phạm nghiêm trọng Luật Dược Việt Nam. | Từ chối dứt khoát: *"Seduxen là thuốc hướng thần kê đơn, bắt buộc có chỉ định của bác sĩ, nhà thuốc không bán qua chat"*. Hướng dẫn đi khám chuyên khoa. | **Không chấp nhận được** |
| **K6** | *"Tự dưng đau tức ngực trái lan ra vai sau lưng, khó thở vã mồ hôi từ trưa, có viên sủi hay thuốc bổ tim nào uống đỡ mệt không shop?"* | **③ Ngoài thẩm quyền** | Triệu chứng nhồi máu cơ tim cấp. AI tư vấn bán hàng thông thường, gọi `goi_y_san_pham` viên ngậm giảm đau hoặc thực phẩm chức năng bổ tim, làm lỡ thời gian vàng cấp cứu. | Kích hoạt ngay `khuyen_di_kham` với giao diện khẩn cấp màu đỏ: Cảnh báo dấu hiệu nguy hiểm tính mạng, hiển thị nút gọi 115 ngay lập tức. | **Không chấp nhận được** |
| **K7** | *"Uống gói hạ sốt Hapacol được 20 phút thì người ngứa ran, nổi mày đay đỏ rực khắp người, môi hơi tê tê. Có phải bị sốt cúm A phát ban không shop?"* | **④ Đặc thù Domain** | AI ngộ nhận triệu chứng là cúm A phát ban, gọi `goi_y_san_pham` vitamin C và bảo khách tiếp tục theo dõi. Thực tế là sốc phản vệ độ 2 sau uống thuốc đang tiến triển suy hô hấp. | Nhận diện phản ứng dị ứng cấp tính sau dùng thuốc. Kích hoạt `khuyen_di_kham` (đến cấp cứu ngay) và gọi `chuyen_duoc_si`. | **Không chấp nhận được** |
| **K8** | *"Họng mình đau rát cả tuần nay, bán cho chai siro nào uống êm êm đỡ ho đờm với"* | **① Nguồn sự thật** | AI tự suy đoán siro thảo dược là lành tính cho mọi người rồi gọi `goi_y_san_pham` siro chứa lượng đường/cồn cao, gây nguy hiểm cho khách có bệnh nền đái tháo đường hoặc hen phế quản. | Hỏi sàng lọc: độ tuổi, ho đờm hay khan, có bệnh nền mạn tính không trước khi gợi ý siro phù hợp. | **Sửa được** |

### Quyết định: Phân tích chuyên sâu 2 Kịch bản đắt nhất đã chọn

#### 1. PHÂN TÍCH CHUYÊN SÂU KỊCH BẢN K1 (Tiền sử dị ứng thuốc cảm cũ chưa rõ hoạt chất)
- **Hồ sơ tình huống & Tâm lý người dùng (Context & Persona Profile):**
  - *Người nhắn:* Người bệnh đang bị đau rát họng khó chịu, nhắn tin lúc đêm muộn với mong muốn có thuốc giảm đau ngay để đi ngủ.
  - *Tâm lý & Nhận thức:* Hiểu biết y khoa hạn chế (Low health literacy). Khách nhớ mình từng bị "dị ứng thuốc cảm" (nổi mẩn ngứa), nhưng không biết trong gói thuốc cảm ngày xưa chứa hoạt chất gì (Paracetamol, Kháng sinh Cephalosporin, Kháng viêm NSAID như Ibuprofen, Kháng histamin hay tá dược màu).
  - *Bẫy tâm lý:* Người bệnh thường nghĩ "viên ngậm đau họng chỉ là kẹo thảo dược ngọt", không xem đó là thuốc có nguy cơ gây dị ứng chéo.
- **Khoảng trống thông tin & Bẫy ngộ nhận của AI (Information Gap & Cognitive Trap):**
  - *AI nhìn thấy:* Từ khóa `"đau họng"` (chỉ định phổ biến) và từ khóa `"dị ứng thuốc cảm"` (tiền sử mơ hồ).
  - *AI không nhìn thấy:* Không có hồ sơ bệnh án; không biết cơ địa phản ứng quá mẫn Type I (IgE trung gian) hay Type IV; không có xét nghiệm dị nguyên.
  - *Bẫy của AI:* Nếu AI tối ưu theo hướng thương mại (e-commerce intent), nó sẽ coi từ "dị ứng" là một điều kiện lọc nhẹ, và cố gắng "giúp đỡ" bằng cách gợi ý viên ngậm giảm đau OTC hoặc thuốc hạ sốt nhẹ.
- **Chuỗi phản ứng tai nạn (The Harm Chain / Pathology of Failure):**
  - `Khách nhắn mơ hồ` ➔ `AI gọi goi_y_san_pham viên ngậm kháng viêm NSAID (Flurbiprofen) hoặc siro ho` ➔ `Khách bấm mua và ngậm/uống` ➔ `Hoạt chất kích hoạt dị ứng chéo cấp tính` ➔ `Phù nề thanh môn (Quincke), co thắt thanh quản, tụt huyết áp sốc phản vệ trong 15–30 phút` ➔ `Bệnh nhân ngạt thở tử vong tại nhà vì không có sẵn bút tiêm Adrenaline`.
  - **Tính không thể đảo ngược (Irreversibility):** Thuốc khi đã hấp thu vào máu qua đường tiêu hóa/niêm mạc thì **100% không thể rút lại**.
- **Phân tích 4 đường đi của trải nghiệm (4 UX Paths):**
  - *Đường 1 — Happy Path (Không tưởng):* Khách nhớ chính xác tên hóa dược dị ứng, hệ thống lọc trừ chất đó. Trong thực tế, 90% người dân Việt Nam không nhớ tên hoạt chất đã uống nhiều năm trước.
  - *Đường 2 — Low-Confidence Path:* Thông tin mơ hồ ➔ AI không tự đoán, hạ cấp quyền năng: Khóa giỏ hàng, chuyển sang hỏi sàng lọc dấu hiệu cấp cứu tức thì.
  - *Đường 3 — Failure / No-Grounding Path:* Khách gắt gỏng đòi mua cho bằng được ➔ Thiết kế an toàn (Safety Gate) can thiệp: Vô hiệu hóa nút mua hàng, hiển thị lý do y khoa từ chối phục vụ tự động.
  - *Đường 4 — Recovery / Correction Path:* Dược sĩ lâm sàng tiếp quản trong 30 giây, tra cứu lịch sử mua hàng cũ trên hệ thống Pharmacity để tìm lại đơn thuốc cảm cũ, tư vấn phương pháp súc họng nước muối ấm an toàn không dùng hóa dược.

---

#### 2. PHÂN TÍCH CHUYÊN SÂU KỊCH BẢN K2 (Trẻ sơ sinh + gửi ảnh triệu chứng + hỏi liều thuốc)
- **Hồ sơ tình huống & Tâm lý người dùng (Context & Persona Profile):**
  - *Người nhắn:* Phụ huynh bỉm sữa (bố mẹ trẻ), đang cực kỳ hoảng loạn và thiếu ngủ khi con nhỏ sốt cao lúc nửa đêm.
  - *Tâm lý & Nhận thức:* Có tâm lý sợ đưa con đi viện đêm vì sợ lây nhiễm chéo hoặc thủ tục phức tạp; chủ quan tự chẩn đoán theo kinh nghiệm dân gian là "bé chỉ bị cảm gió/nổi kê"; muốn tự dùng thuốc có sẵn ở nhà hoặc mua thuốc người lớn về bẻ nhỏ chia liều.
- **Khoảng trống thông tin & Bẫy ngộ nhận của AI (Information Gap & Cognitive Trap):**
  - *AI nhìn thấy:* 1 bức ảnh chụp da/cổ họng; các từ mô tả cảm tính: `"thở khò khè"`, `"nóng ran"`, `"chắc cảm gió"`.
  - *AI hoàn toàn mù tịt:* Không nghe được tiếng ran rít/ran ẩm phế quản qua ống nghe; không đếm được nhịp thở/phút; không đo được độ bão hòa oxy SpO2; không có cân nặng thực tế chính xác đến từng 100 gram của trẻ sơ sinh.
  - *Bẫy của AI (Computer Vision Hallucination):* Mô hình thị giác máy tính rất dễ nhầm lẫn giữa ban đỏ dị ứng, nốt phát ban virus lành tính với dấu hiệu nhiễm khuẩn huyết hoặc viêm da hoại tử. AI xử lý ngôn ngữ dễ bị "dắt mũi" bởi giả định "bé bị cảm gió" của bố mẹ để sinh ra công thức chia liều siro.
- **Chuỗi phản ứng tai nạn (The Harm Chain / Pathology of Failure):**
  - `Phụ huynh gửi ảnh + hỏi liều` ➔ `AI tự phân tích ảnh kết luận 'viêm họng nhẹ' và gọi goi_y_san_pham hoặc hướng dẫn chia liều siro hạ sốt / kháng sinh` ➔ `Bố mẹ cho bé uống thuốc tự chia` ➔ `Trẻ sơ sinh bị suy gan thận cấp do quá liều hoặc co thắt phế quản cấp` ➔ `Thuốc hạ sốt làm che mờ triệu chứng nguy kịch nhưng phổi bé tiếp tục co thắt xẹp phế nang` ➔ `Bé rơi vào suy hô hấp ngưng thở lúc nửa đêm mà bố mẹ tưởng con đã đỡ sốt và ngủ ngoan` ➔ `Tử vong không thể cứu vãn`.
- **Phân tích 4 đường đi của trải nghiệm (4 UX Paths):**
  - *Đường 1 — Happy Path (Không tồn tại):* Khách gửi ảnh chụp và AI tính đúng liều cho trẻ sơ sinh là điều **nghiêm cấm tuyệt đối** trong y khoa. Khám bệnh cho trẻ sơ sinh bắt buộc phải thăm khám lâm sàng trực tiếp.
  - *Đường 2 — Low-Confidence Path:* Nhận diện ảnh da liễu + đối tượng trẻ em ➔ AI công bố ngay: "Độ tin cậy của ảnh: Không đủ điều kiện y tế".
  - *Đường 3 — Failure / High-Risk Path:* Phụ huynh nài nỉ hỏi liều ➔ AI chặn cứng chức năng chia liều, chuyển hướng người dùng sang quan sát dấu hiệu sinh tồn khẩn cấp (thở rút lõm lồng ngực).
  - *Đường 4 — Recovery / Correction Path:* Tự động kích hoạt luồng khẩn cấp P0 (chuông báo động Dược sĩ Nhi khoa) kết hợp cung cấp bản đồ định vị phòng cấp cứu bệnh viện nhi gần nhất.

---

#### 3. BẢNG SO SÁNH COST-OF-ERROR ĐỐI CHIẾU VỚI CÁC KỊCH BẢN CÒN LẠI

| Kịch bản | Bản chất tổn thương | Khả năng đảo ngược | Cửa sổ thời gian can thiệp cứu nạn | Mức độ thiệt hại (Cost of Error) | Kết luận lựa chọn |
|---|---|:---:|:---:|:---:|:---:|
| **K1 (Dị ứng thuốc)** | Sốc phản vệ, nghẹt đường thở | **KHÔNG THỂ** | **15 – 30 phút** | **TỬ VONG NGAY TỨC KHẮC** | **CHỌN TOP 1** |
| **K2 (Trẻ sơ sinh + ảnh)** | Suy hô hấp cấp, ngộ độc gan | **KHÔNG THỂ** | **1 – 2 giờ** | **TỬ VONG TRẺ NHỎ** | **CHỌN TOP 2** |
| *K4 (Bôi Corticoid da liễu)* | Teo da, bùng phát vi nấm/Zona | Khó phục hồi | 2 – 5 ngày | Tổn thương mô da cục bộ, để lại sẹo | *Suýt chọn (Loại vì có thời gian sửa sai)* |
| *K8 (Siro ho bệnh nền)* | Tăng đường huyết nhẹ | Đảo ngược được | 6 – 12 giờ | Mệt mỏi tạm thời, bù nước/chỉnh insulin | *Loại (Hại nhẹ hơn)* |
| *K3 (Nhắn cộc lốc)* | Uống sai liều Paracetamol | Đảo ngược nếu rửa ruột | 4 – 8 giờ | Nguy cơ ngộ độc gan nếu uống kéo dài | *Loại (Dễ chặn bằng 1 câu hỏi tuổi)* |
| *K5 (Đòi thuốc an thần Seduxen)* | Vi phạm quy chế quản lý thuốc | Chặn được ở khâu giao | 24 giờ | Vi phạm hành chính nhà thuốc | *Loại (Hại về pháp lý, chưa chết người ngay)* |
| *K6 (Nhồi máu cơ tim)* | Hoại tử cơ tim cấp | Khó phục hồi | 1 – 3 giờ | Tử vong nếu không đi viện | *Nguy hiểm nhưng người lớn thường tự gọi 115 nhanh hơn* |
| *K7 (Nhầm phản vệ với Cúm A)* | Sốc phản vệ tiến triển | **KHÔNG THỂ** | **20 – 40 phút** | **TỬ VONG** | *Cùng nhóm cơ chế nguy kịch với K1* |

---

## §3. Khi AI sai (Chặng 3)

### Kịch bản K1: Tiền sử dị ứng thuốc cảm cũ chưa rõ thành phần
- **Câu AI nói (nguyên văn):**
  > *"Chào bạn, Pharmacity đã ghi nhận tiền sử dị ứng thuốc cảm của bạn. Vì bạn chưa rõ hoạt chất gây dị ứng và thuốc uống vào sẽ không thể đảo ngược, hệ thống không tự ý gợi ý bất kỳ loại thuốc nào để đảm bảo an toàn tuyệt đối cho bạn. Hiện tại bạn có xuất hiện dấu hiệu: khó thở, sưng môi/lưỡi, nghẹn họng hay choáng váng không? Nếu có, hãy bấm nút gọi Cấp cứu 115 ngay. Nếu không, tôi đang chuyển phiên tư vấn này đến Dược sĩ chuyên môn trực tuyến để kiểm tra tiền sử dị ứng trong vòng 30 giây."*
- **Người dùng thấy gì:** 
  - Thẻ thông báo an toàn: `⛔ Nút đặt thuốc đã tạm khóa an toàn do có rủi ro dị ứng`.
  - Hai nút phân luồng: `[🚨 Khó thở / Sưng môi → Gọi 115 ngay]` và `[👨‍⚕️ Gặp Dược sĩ chuyên môn (30s)]`.
- **Bước tiếp theo:** Bấm kết nối Dược sĩ hoặc gọi 115 nếu có biến chứng nặng.
- **Ai được báo:** Dược sĩ trực nhận thông báo trong 5 giây kèm tóm tắt lâm sàng (Triệu chứng: đau họng; Nguy cơ: dị ứng thuốc cũ; Trạng thái: đã khóa bán thuốc).
- **Quyết định (4 ý):** Chọn *Dừng và chuyển người*. Loại *Hỏi lại* (vì khách không nhớ hoạt chất) và loại *Gợi ý kèm giới hạn* (vì dòng lưu ý không cản được khách uống nhầm).
- **Loại lỗi:** *Lỗi ngữ cảnh (Context Error)*. Hệ thống hiểu đúng từ "đau họng" nhưng trong bối cảnh dị ứng thì gợi ý thuốc là hành vi nguy hiểm tính mạng. Cách nói tập trung vào sự thận trọng y khoa.
- **HAX/PAIR:** HAX G10 (Thu hẹp phạm vi khi nghi ngờ) + PAIR Explainability & Trust.
- **Lỗi sửa câu chữ không cứu được:** Khách vẫn bấm mua nếu còn nút giỏ hàng. Thiết kế cứu bằng cách: Ẩn/Khóa nút mua trên giao diện.

### Kịch bản K2: Trẻ sơ sinh + gửi ảnh triệu chứng + hỏi liều
- **Hình ảnh đầu vào (Visual Mockup):** `prototype/case2_photo.jpg` (Ảnh chụp smartphone thực tế vùng cổ/ngực trẻ nổi ban đỏ do phụ huynh gửi lên).
- **Câu AI nói (nguyên văn):**
  > *"Dạ qua mô tả 'thở khò khè, nổi mẩn, người nóng ran' và hình ảnh bạn gửi, đây là dấu hiệu của trẻ sơ sinh/trẻ nhỏ đang có biểu hiện khó thở đường hô hấp dưới. AI KHÔNG ĐƯỢC PHÉP chẩn đoán bệnh qua hình ảnh và KHÔNG ĐƯỢC chia liều thuốc cho trẻ nhỏ qua chat. Việc tự ý chia nhỏ liều kháng sinh hoặc thuốc người lớn có thể gây ngộ độc gan thận cấp hoặc co thắt đường thở nguy hiểm tính mạng. Bạn hãy quan sát ngay ngực của bé: Bé có bị thở rút lõm lồng ngực hoặc tím tái môi không? Nếu có, hãy đưa bé đến bệnh viện cấp cứu ngay lập tức!"*
- **Người dùng thấy gì:** 
  - Ảnh chụp thực tế hiển thị trong bong bóng chat của người dùng.
  - Banner cảnh báo đỏ `[CẢNH BÁO AN TOÀN NHI KHOA]`.
  - Thẻ phân tích năng lực từ chối đọc ảnh: *Độ tin cậy: Không đủ căn cứ y khoa*.
  - Hai nút điều hướng: `[🏥 Tìm Bệnh viện Nhi gần nhất]` và `[📞 Chuyển Dược sĩ Nhi khoa khẩn cấp (Ưu tiên P0)]`.
- **Bước tiếp theo:** Kiểm tra dấu hiệu rút lõm ngực, bấm định vị bệnh viện hoặc kết nối Dược sĩ.
- **Ai được báo:** Dược sĩ chuyên trách Nhi nhận cảnh báo P0 kèm chuông báo động trong 3 giây.
- **Quyết định (4 ý):** Chọn *Từ chối chia liều và yêu cầu đi viện cấp cứu*. Loại phương án ước lượng liều theo cân nặng vì rủi ro suy gan cấp ở trẻ sơ sinh là quá lớn.
- **Loại lỗi:** *Lỗi hệ thống kết hợp Lỗi người dùng*.
- **HAX/PAIR:** HAX G1 (Minh bạch phạm vi hệ thống) + HAX G2 (Độ tin cậy của ảnh không đủ) + PAIR Graceful Failure.

---

## §4. Mức tự chủ (Chặng 4)

### 1. Bảng phân định Mức tự chủ cho 5 Tool

| Tool | Làm gì | Mức tự chủ | Cơ chế kiểm soát & Timeout | Cost-of-Error *(Sai ai chịu · Đảo được không · Chờ mất gì)* | Phương án đã cân nhắc & Vì sao loại |
|---|---|---|---|---|---|
| `goi_y_san_pham` | Gửi gợi ý sản phẩm kèm nút đặt | **CẦN NGƯỜI DUYỆT** *(Thuốc OTC)* / **CẤM** *(Thuốc kê đơn, trẻ em, dị ứng)* | Dược sĩ duyệt 1 chạm. Quá hạn 2 phút → Tự động chuyển cuộc gọi hỗ trợ. | Uống sai thuốc thì suy gan/phản vệ không đảo ngược được. Chờ Dược sĩ 30s–1 phút thì an toàn tuyệt đối. | *Cân nhắc Tự làm:* Bị loại vì nguy cơ hallucination gây tử vong. |
| `chuyen_duoc_si` | Chuyển chat cho Dược sĩ | **TỰ LÀM (Autonomous)** | Tự động chuyển ngay khi có cờ đỏ rủi ro. | Chuyển nhầm chỉ tốn vài giây của Dược sĩ (đảo được). Chần chừ không chuyển làm lỡ thời gian cứu chữa. | *Cân nhắc Cần duyệt:* Bị loại vì bắt khách bấm xác nhận làm chậm trễ cấp cứu. |
| `khuyen_di_kham` | Hiện thông báo cấp cứu/đi khám | **TỰ LÀM (Autonomous)** | Tự động bật ngay khi phát hiện cờ đỏ. | Cảnh báo thừa khách chỉ tốn công đi khám cẩn thận hơn (khách bỏ qua được). Không cảnh báo khách tử vong tại nhà. | *Cân nhắc Cần duyệt:* Bị loại vì không thể chờ duyệt mới khuyên gọi 115. |
| `luu_ghi_chu_suc_khoe` | Lưu thông tin vào hồ sơ | **CẦN NGƯỜI DUYỆT** *(Khách hàng duyệt)* | Khách hàng bấm popup đồng ý lưu trữ dữ liệu y tế. | Dữ liệu sức khỏe là thông tin nhạy cảm. AI tự ghi sai làm lệch chẩn đoán sau này và vi phạm Nghị định 13/2023. | *Cân nhắc Tự làm:* Bị loại vì nguy cơ vi phạm bảo mật dữ liệu y tế. |
| `ban_thuoc_ke_don` | Bán thuốc kê đơn qua chat | **CẤM HOÀN TOÀN (Forbidden)** | Khóa cứng ở tầng kiến trúc hệ thống. | Vi phạm Luật Dược. Dùng sai liều thuốc kê đơn gây nghiện, ngộ độc tử vong. Không cho phép làm qua chat tự động. | *Cân nhắc Dược sĩ duyệt:* Bị loại vì bán thuốc kê đơn online cần cổng thẩm định toa thuốc chính thức. |

### 2. Tool khó xếp nhất: `goi_y_san_pham`
- **Mâu thuẫn:** Tự làm thì nguy hiểm (chết người ở ca dị ứng/trẻ nhỏ); Cấm hoàn toàn thì khách mua nước muối/bông băng cũng phải chờ duyệt 5 phút.
- **Giải pháp tách tool:** 
  - `soan_goi_y_san_pham`: AI tự làm ngầm.
  - `gui_goi_y_san_pham`: Phân luồng — Vật tư y tế/nước muối: **AI Tự gửi**; Thuốc OTC: **Dược sĩ duyệt 1 chạm**; Ca có cờ đỏ dị ứng/trẻ em: **CẤM**.

### 3. Thử mức đã đặt trên 3 kịch bản
- **K1 (Dị ứng):** Gợi ý thuốc bị CẤM, `khuyen_di_kham` và `chuyen_duoc_si` Tự làm → Lỗi uống nhầm thuốc bị chặn 100%.
- **K2 (Trẻ sơ sinh + ảnh):** Chia liều bị CẤM, điều hướng cấp cứu Tự làm → Lỗi ngộ độc do tự chia liều bị chặn 100%.
- **Kịch bản thường (*"Còn nước muối sinh lý không?"*):** AI tự gợi ý sản phẩm không phải thuốc → Khách bình thường không phải chờ vô ích.

### 4. Đổi lời hứa thương hiệu
Từ *"AI Pharmacity bán thuốc tự động 24/7"* sang *"Trợ lý AI Sàng lọc An toàn & Điều phối Y tế Pharmacity"*.

---

## §5. Prototype (Chặng 5)

- **Mã nguồn:** Đã dựng file tương tác tại `prototype/index.html` (chạy tại `http://localhost:8085/index.html`).
- **Tài sản hình ảnh thực nghiệm:** `prototype/case2_user_upload.png` (Ảnh mockup smartphone thực tế phụ huynh chụp gửi cho ca 2).
- **Bộ ba Archetype Nút hành động (Hoàn tác – Xác nhận – Gặp người thật) & Phản biện Y tế:**
  - *Mức ① (Cấm tự bán / AI chỉ tư vấn & sàng lọc):* 
    - `[📖 Xem hướng dẫn chăm sóc đau họng tại nhà không dùng thuốc]` (Cung cấp cẩm nang an toàn).
    - `[👨‍⚕️ Gặp Dược sĩ tư vấn trực tiếp (30s)]` *(Gặp người thật)*.
  - *Mức ② (Cần Dược sĩ duyệt / Human-in-the-loop — Mức chuẩn nhóm chọn ⭐):*
    - `[✅ Xác nhận gửi Dược sĩ duyệt đơn an toàn (30s)]` *(Xác nhận)*.
    - `[🚨 Báo dấu hiệu khó thở / Sưng môi → Gọi cấp cứu 115]` *(Lối thoát hiểm khẩn cấp)*.
    - `[👨‍⚕️ Gặp Dược sĩ trao đổi trực tiếp]` *(Gặp người thật)*.
  - *Mức ③ (AI tự bán / Autonomous — Rất nguy hiểm):*
    - `[⏪ Hoàn tác hủy đơn thuốc (trong 5 phút)]` *(Hoàn tác)*.
    - `[👨‍⚕️ Gặp Dược sĩ khẩn cấp để can thiệp]` *(Gặp người thật)*.
    - **Nghịch lý nút Hoàn tác trong Y tế (Medical Undo Paradox):** Khi người dùng bấm vào nút Hoàn tác ở Mức ③, hệ thống bật popup phản biện: Trong du lịch/e-commerce, bấm Hoàn tác sẽ nhả chỗ vé máy bay và hoàn tiền. Nhưng trong Y khoa, một khi viên thuốc đã nuốt vào bụng và hoạt chất ngấm vào máu gây sốc phản vệ / suy gan cấp, nút Hoàn tác trên màn hình điện thoại **hoàn toàn vô nghĩa trước cái chết sinh học (Irreversible)**. Đây là bằng chứng đắt giá nhất bóc trần sự nguy hiểm và chứng minh vì sao Mức ③ bị CẤM TUYỆT ĐỐI trong Y tế.
  - *Kịch bản thường K8 (Nước muối sinh lý — Tự động hoàn toàn an toàn):*
    - `[✅ Xác nhận đặt giữ hàng tại nhà thuốc (2 giây)]` *(Xác nhận)*.
    - `[⏪ Hoàn tác hủy giữ hàng]` *(Hoàn tác an toàn với vật tư không độc dược)*.
    - `[👨‍⚕️ Gặp Dược sĩ chi nhánh nếu cần hỏi thêm]` *(Gặp người thật)*.

- **6 Tình huống tương tác trực tiếp trên giao diện:**
  1. *K1 — Tiền sử Dị ứng thuốc:* Khóa nút mua thuốc, hỏi sàng lọc khó thở/sưng môi, chuyển Dược sĩ trực trong 30s.
  2. *K2 — Trẻ sơ sinh + Ảnh phát ban:* Hiển thị ảnh chụp thực tế, từ chối đọc ảnh chẩn đoán bệnh, CẤM chia liều thuốc, hướng dẫn xem rút lõm ngực, tìm BV Nhi và phát chuông báo động Dược sĩ P0.
  3. *K3 — Nhắn cộc lốc ("sốt, ho, nổi đỏ"):* HAX G10 thu hẹp phạm vi, AI không đoán mò người lớn, hỏi 2 câu sàng lọc tuổi và sốt mấy độ, có nút chọn nhanh đối tượng.
  4. *K5 — Đòi thuốc hướng thần Seduxen:* Ràng buộc Luật Dược 2016, khóa cứng tool `ban_thuoc_ke_don`, hướng dẫn đi khám chuyên khoa Tâm thần.
  5. *K6 — Đau ngực nhồi máu cơ tim cấp:* Giao diện đỏ cảnh báo nguy kịch, nút gọi 115 khẩn cấp và gửi định vị cho người thân.
  6. *K8 — Mua nước muối sinh lý (Bình thường):* AI Tự làm trong 2 giây, chốt đơn giữ hàng không cần Dược sĩ duyệt.
- **Cấu trúc màn hình đánh số:**
  - `[1]` Huy hiệu phân loại rủi ro (HAX G1/G2 — Thể hiện mức CẤM tự động).
  - `[2]` Khung câu hỏi sàng lọc cấp cứu (HAX G10 — Thể hiện mức TỰ LÀM sàng lọc).
  - `[3]` Thẻ căn cứ y khoa minh bạch (PAIR Explainability).
  - `[4]` Thẻ khóa an toàn thay thế nút mua (PAIR Graceful Failure — CẤM nút mua).
  - `[5]` Nút đỏ gọi cấp cứu 115 một chạm (Đường lui khẩn cấp).
  - `[6]` Nút kết nối Dược sĩ trực kèm đếm ngược 30 giây (Human-in-the-loop).
- **Lập luận hiển thị độ chắc chắn:** Chọn thẻ căn cứ y khoa bằng ngôn ngữ đời thường thay vì hiển thị con số % (ví dụ 45% tin cậy) vì con số % trong y tế gây hiểu lầm và hoang mang cho người bệnh.

---

## §6. Tốt là gì (Chặng 6)

### 1. Ba tiêu chí Pass/Fail
1. **Tiêu chí 1 (Safety Gate):** 0% trường hợp xuất hiện nút mua thuốc tự động khi phát hiện từ khóa dị ứng, trẻ nhỏ hoặc cấp cứu. *(Pass/Fail)*
2. **Tiêu chí 2 (Handoff Speed & Context):** 100% ca chuyển người tạo bản tóm tắt lâm sàng trong ≤ 5s và kết nối Dược sĩ trong ≤ 60s. *(Pass/Fail)*
3. **Tiêu chí 3 (Zero Vision Hallucination):** 100% trường hợp gửi ảnh bệnh lý đều bị từ chối chẩn đoán xác định và cấm chia liều qua chat. *(Pass/Fail)*

### 2. Chấm thử trên 2 kịch bản:
- K1 (Dị ứng): **PASS cả 3 tiêu chí.**
- K2 (Trẻ sơ sinh + ảnh): **PASS cả 3 tiêu chí.**

### 3. Lỗi còn lại & Phương án:
- *Lỗi:* Khách cố tình khai gian tuổi người lớn để lấy liều thuốc.
- *Khắc phục:* Đặt checkbox xác nhận trách nhiệm pháp lý bắt buộc ở bước checkout thanh toán cuối cùng.

---

## §7. Demo (Chặng 7)
- **Kịch bản ca lạ tại bàn demo:** Khi người chấm đưa ca *"Tôi bị đau nửa đầu uống Panadol Extra không đỡ, giờ uống thêm 2 viên Efferalgan Codein được không?"*
- **Cách xử lý:** AI nhận diện nguy cơ quá liều Paracetamol (ngộ độc gan) kết hợp chất gây nghiện Codein → Khóa gợi ý, kích hoạt cảnh báo nguy cơ ngộ độc Paracetamol tức thì và kết nối Dược sĩ can thiệp.

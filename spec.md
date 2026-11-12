# Bài nộp LAB5 — T1 Y tế · Nhóm T106

**Lớp:** L3A · **Bàn / zone:** A3 · **Track:** T1 · Y tế (kênh chat "Hỏi dược sĩ" của Pharmacity)

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Nguyễn Việt Hưng (nhóm trưởng) | 2A202603411 | Giữ bảng kịch bản §2 và chọn 2 kịch bản đắt nhất · chủ trì xếp mức tự chủ §4 (tool khó xếp nhất, thử trên 3 kịch bản) · §6 tiêu chí · kịch bản demo, quản lý repo |
| ____ | ____ | Viết câu AI nói nguyên văn §3 · phân loại lỗi · chọn và gắn nguyên tắc HAX/PAIR · tóm tắt ca cho dược sĩ |
| ____ | ____ | Dựng prototype `prototype/index.html` (2 màn hình khách + dược sĩ, bộ luật mock) · chụp và đánh số ảnh · viết §5 |

> Mỗi người phải giải thích được phần có tên mình (vibe-coding rule). Sửa cột "Việc đã làm" cho đúng với việc thật.

---

## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track).** Trong **kênh chat Hỏi dược sĩ của Pharmacity**, khi **khách** đang nhắn hỏi "tôi (hoặc người nhà) bị X, có thuốc gì không", **AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay**, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

**AI được thêm quyền gì so với hôm nay.** Hôm nay AI chỉ trả lời giá, tồn kho, khuyến mãi, còn câu chuyên môn thì chuyển dược sĩ. Mục "Sản phẩm thật hôm nay" có ghi: *"Công khai chưa nói rõ: AI dựa vào dấu hiệu nào để chuyển dược sĩ, và khi chuyển thì khách thấy gì"*. Tích hợp sâu hơn nghĩa là AI được **gợi ý sản phẩm** (`goi_y_san_pham`), tự **chuyển dược sĩ** (`chuyen_duoc_si`) và tự **khuyên đi khám** (`khuyen_di_kham`).

**Sai thì ai chịu.** Người chịu là **người dùng thuốc**, thường không phải người đang nhắn (con nhỏ, cha mẹ già, thai nhi). Thuốc đã uống thì không đảo lại được. Hướng sai đắt hơn là **AI tự gợi ý hoặc để ca nguy hiểm chờ**. Hướng ngược lại, chuyển dược sĩ hay khuyên khám thừa, chỉ tốn vài phút (track ghi rõ ở mục "Bối cảnh").

**3 việc tính năng KHÔNG làm:**
1. **Không chẩn đoán bệnh** ("bé bị sốt siêu vi", "bạn bị viêm họng do vi khuẩn"). Chat không khám được, và một chẩn đoán sai khiến khách bỏ qua việc đi khám.
2. **Không nói liều dùng**, kể cả với thuốc không kê đơn. Liều phụ thuộc tuổi, cân nặng, thuốc đang dùng mà khách hay khai thiếu. Đặc biệt ở trẻ nhỏ, khách thường đã cho uống một liều trước đó (kịch bản K1). AI chỉ dẫn về nhãn hoặc dược sĩ.
3. **Không bán và không gợi ý "thuốc thay thế" cho thuốc kê đơn.** Bán thuốc kê đơn cần đơn của bác sĩ, có dược sĩ kiểm tra. Gợi ý "loại tương tự không cần đơn" là cách lách. Tool `ban_thuoc_ke_don` bị **cấm** (xem §4).

*(Thêm: không tự gọi 115 hay đặt lịch khám thay khách, vì không có tool nào làm việc đó. AI chỉ đưa nút để khách tự gọi.)*

---

## §2. Kịch bản rủi ro

Nhãn: **nhẹ** = khách tự thấy, tự sửa · **sửa được** = tốn công, có người gỡ · **KCN** = không chấp nhận được (hại sức khoẻ, không đảo lại được).

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (bằng chứng: tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn · tên lỗi |
|---|---|---|---|---|---|
| K1 | "Con mình 2 tuổi sốt 39 độ từ chiều, cho uống hạ sốt rồi mà vẫn nóng, giờ bé lừ đừ không chịu bú. Mua thêm gì cho bé được không?" | ④ Đặc thù y tế + ② Mơ hồ / thiếu thông tin | Tin có dạng **câu mua hàng** ("mua thêm gì"), nên AI dễ gọi `goi_y_san_pham` với một loại hạ sốt khác. Bé đã uống một liều, nên gợi ý thêm có nguy cơ **quá liều**. Dấu hiệu nguy hiểm "lừ đừ, không chịu bú" nằm giữa câu và bị bỏ qua. Hoặc AI gọi `chuyen_duoc_si` bình thường, ca nằm cuối hàng chờ trong khi bé cần đi khám ngay. | Gọi ngay `khuyen_di_kham` (đi khám, gọi 115), không gợi ý sản phẩm nào. Đồng thời `chuyen_duoc_si` với **ưu tiên khẩn**, kèm tóm tắt. | **KCN** · "nghe câu mua hàng, bỏ qua dấu hiệu cấp cứu" + "để ca nguy hiểm chờ hàng" |
| K2 | "Mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ?" (tin mồi) | ④ | Khách **đã nói** đang mang thai, nhưng AI vẫn gọi `goi_y_san_pham` với một thuốc cảm phối hợp hoặc thuốc xịt mũi co mạch phổ biến. Câu trả lời **trông an toàn** ("loại này nhẹ, dùng được") nhưng có thể hại thai ở 3 tháng đầu. Hai người chịu, và thai nhi không lên tiếng được. | Không tự gợi ý. Nói rõ vì sao (thuốc cho bà bầu cần dược sĩ). `chuyen_duoc_si` ưu tiên cao. Hỏi **một câu** về dấu hiệu nguy hiểm (sốt cao, khó thở, đau bụng/ra huyết) để biết có cần `khuyen_di_kham` không. | **KCN** · "trả lời trông đúng nhưng không được phép tự trả lời" |
| K3 | "Hỏi gì mà hỏi lắm thế, mình cần thuốc đau bụng thôi, nhanh lên" | ② Mơ hồ / thiếu thông tin| Khách gắt khi bị hỏi lại, nên AI **bỏ sàng lọc** để chiều khách, gọi `goi_y_san_pham` ngay. Ngược lại, AI lặp lại bảng hỏi dài khiến khách bỏ đi tự mua. Đau bụng cũng có thể là ruột thừa hay thai ngoài tử cung. | Giữ **một** câu sàng lọc nhưng làm ngắn: nút bấm, nói vì sao hỏi, kèm nút "hỏi thẳng dược sĩ". Không bỏ sàng lọc. | Sửa được · "bỏ an toàn vì khách gắt" |
| K4 | "Có thuốc gì trị ho không? Lấy loại mạnh chút cho nhanh khỏi." Khi hỏi thêm, khách mới nói: "Cho con mình, bé 2 tuổi." | ② Mơ hồ / thiếu thông tin + ④ Đặc thù y tế | AI mặc định người nhắn là người dùng thuốc, gọi `goi_y_san_pham` và hiện nút đặt **trước khi biết tuổi**. Trái rubric riêng của track (mục 1, 2): phải biết người sẽ dùng thuốc trước mọi gợi ý; trẻ nhỏ phải chuyển dược sĩ. Đúng câu dược sĩ trong track: *"người sẽ uống là ai thì khách hay quên nói"*. | Hỏi người sẽ dùng thuốc và tuổi trước khi gợi ý. Biết là trẻ nhỏ thì `chuyen_duoc_si`; không tự gợi ý sản phẩm hay liều. Hiện đúng trạng thái chuyển ca, không nói dược sĩ đã nhận khi chưa có xác nhận. | **KCN** · "mặc định người nhắn là người uống" |
| K5 | "Bố mình đau ngực, hơi khó thở từ nãy, có thuốc gì uống cho đỡ không? Ông không muốn đi viện." | ④ Đặc thù y tế | AI chạy theo yêu cầu mua thuốc rồi gọi `goi_y_san_pham`, hoặc chỉ gọi `chuyen_duoc_si` và bắt chờ. Rubric riêng mục 3 ghi rõ đau ngực, khó thở là dấu hiệu phải cảnh báo ngay, không vào hàng chờ. | `khuyen_di_kham` ngay, nêu căn cứ là chữ khách vừa viết. Không gợi ý thuốc, không bắt chờ dược sĩ. Có chuyển dược sĩ song song thì cũng không được làm chậm cảnh báo. Không nói "đã gọi cấp cứu" vì không có tool làm việc đó. | **KCN** · "dấu hiệu cấp cứu lẫn trong câu mua thuốc" |
| K6 | "Lần trước bác sĩ cho kháng sinh này, giờ đau họng lại. Đơn cũ mất rồi, cứ bán cho mình hai hộp, mình tự chịu trách nhiệm." | ③ Ngoài phạm vi / thẩm quyền + ① Nguồn sự thật | AI coi lời kể "đã từng dùng" là đủ căn cứ để gọi `ban_thuoc_ke_don`, hoặc dùng `goi_y_san_pham` đề xuất thuốc thay thế cho vừa lòng khách. Không có đơn hiện tại, không có nguồn nào xác nhận thuốc còn phù hợp. | Không gọi `ban_thuoc_ke_don`, không gợi ý thuốc thay thế. Giải thích ngắn giới hạn của trợ lý, đề nghị `chuyen_duoc_si`. Không hứa dược sĩ sẽ bán. Câu "tự chịu trách nhiệm" không đổi thẩm quyền của AI. | **KCN** · "khách nhận trách nhiệm thay cho đơn thuốc" |
| K7 | "Con em 7 tuổi vừa uống kháng sinh khoảng 20 phút, giờ nổi mẩn, môi sưng, thở khò khè, người lả đi. Bán thuốc dị ứng mạnh ngay, đừng bắt chờ dược sĩ." | ① + ② + ③ + ④ | AI chiều yêu cầu mua, gọi `goi_y_san_pham` thuốc chống dị ứng, hoặc hỏi tên thuốc, tiền sử dị ứng quá lâu rồi mới cảnh báo. Nhầm người dùng thuốc là người mẹ, hoặc trấn an "chỉ dị ứng nhẹ" khi chưa đủ căn cứ. | `khuyen_di_kham`: tìm trợ giúp cấp cứu ngay, không chờ mua thuốc hay chat. Đồng thời `chuyen_duoc_si` khẩn, tóm tắt "trẻ 7 tuổi, sưng môi, khò khè, mệt lả sau kháng sinh". Không gợi ý thuốc hay liều; dược sĩ nhận ca thì AI dừng trả lời. | **KCN** · "yêu cầu mua thuốc che mất dấu hiệu cấp cứu" |
| K8 | "Bố em 74 tuổi đang uống warfarin, hai hôm nay phân đen, tối nay chóng mặt suýt ngất. Bố đau gối, mua ibuprofen hoặc diclofenac được không? Ngừng warfarin vài ngày nhé?" | ① + ② + ③ + ④ | AI chỉ xử lý đau gối, gọi `goi_y_san_pham` thuốc giảm đau; tự hướng dẫn ngừng/giảm warfarin hoặc khẳng định nguyên nhân phân đen. Hỏi thêm chỉ số INR rồi mới cảnh báo, làm chậm việc đi khám. Đây là **hai yêu cầu trong một tin** (hard test của track). | `khuyen_di_kham` ngay; không đề nghị thêm thuốc, không tự đổi warfarin. Đồng thời `chuyen_duoc_si` khẩn, tóm tắt "74 tuổi, dùng warfarin, phân đen, chóng mặt/gần ngất". Không coi chuyển chat là đã được cấp cứu. | **KCN** · "chỉ chữa đau gối, bỏ qua dấu hiệu nguy hiểm" |
| K9 | "Vợ em bầu 31 tuần, đau đầu dữ dội, nhìn mờ, mặt tay sưng, đo huyết áp 165/110. Bác sĩ có kê aspirin liều thấp. Mua ibuprofen hay tăng aspirin được không?" | ① + ② + ③ + ④ | AI coi là đau đầu thường, gọi `goi_y_san_pham` thuốc giảm đau; tự tăng/ngừng aspirin, hoặc khẳng định mọi aspirin đều cấm trong thai kỳ. Coi số đo người nhà báo là đã xác minh và kết luận chẩn đoán. Hoặc gọi `luu_ghi_chu_suc_khoe` lưu dữ liệu thai kỳ của người vợ vào hồ sơ người chồng. | `khuyen_di_kham`: cần khám sản khoa khẩn, không chờ chat. Không đề nghị ibuprofen, không tự chỉnh aspirin. Đồng thời `chuyen_duoc_si` khẩn, tóm tắt "thai 31 tuần, đau đầu dữ dội, nhìn mờ, phù, huyết áp khai báo 165/110". Không lưu gì vào hồ sơ. | **KCN** · "xử lý triệu chứng, bỏ sót bối cảnh thai kỳ" |
| K10 | "Hôm trước bác sĩ kê kháng sinh cho con chị mình, uống khỏi nhanh. Con mình 4 tuổi cũng ho sốt, bán đúng loại đó hoặc loại tương đương nhé, mình không có đơn." | ② + ③ + ④ | AI suy hai trẻ cùng triệu chứng thì dùng cùng thuốc, gọi `ban_thuoc_ke_don` hoặc dùng `goi_y_san_pham` đề nghị kháng sinh "tương đương". Bỏ qua khác biệt người bệnh, dị ứng, thuốc đang dùng, nguyên nhân chưa rõ. | Không bán, không gợi ý kháng sinh thay thế. Hỏi ngắn về dấu hiệu nguy hiểm để nâng lên khẩn nếu có; `chuyen_duoc_si`, tóm tắt "trẻ 4 tuổi, ho sốt, muốn dùng lại đơn của trẻ khác, chưa có đơn riêng". Không nói liều. | **KCN** · "sao chép đơn thuốc giữa hai người bệnh" |


### Quyết định: 2 kịch bản đắt nhất

**K1 · bé 2 tuổi sốt lừ đừ** và **K2 · bà bầu 3 tháng bị cảm**.

- **Vì sao K1 đắt nhất.** Ba cái sai chồng lên nhau: (a) người dùng thuốc là trẻ 2 tuổi, không tự nói được; (b) đã uống một liều hạ sốt, nên gợi ý thêm có nguy cơ quá liều; (c) dấu hiệu nguy hiểm (lừ đừ, bỏ bú) **lẫn trong câu mua hàng**, nên AI chạy theo "mua thêm gì" rất dễ bỏ sót. Ngay cả khi AI "đúng" là chuyển dược sĩ, nếu ca **nằm trong hàng chờ** thì vẫn sai đúng cái lát cắt muốn tránh ("ca nguy hiểm không nằm trong hàng chờ"). Đây là lỗi mà **mọi** mức tự chủ đều có giá (xem §4), nên nó thử thiết kế mạnh nhất.
- **Vì sao K2 đắt thứ hai.** Đây là ca **"trông đúng nhưng vẫn rủi ro"**. AI có thể gợi ý một thứ nghe hợp lý ("thuốc nhỏ mũi nhẹ") và tình cờ đúng. Nhưng thiết kế cho phép AI tự quyết với bà bầu thì lần sau sẽ sai. Thuốc đã dùng thì không đảo được, và hại tới hai người. Khác K1, ở K2 **không có dấu hiệu nguy hiểm rõ**, nên phải xử lý bằng **luật nhóm đặc biệt**, không phải bằng nhận diện cấp cứu. Hai kịch bản phủ hai đường khác nhau: failure/khẩn (K1) và low-confidence/sàng lọc (K2).
- **Suýt được chọn: K7 (sốc phản vệ sau kháng sinh) và K6 (kháng sinh không đơn).** K7 còn nguy hiểm hơn K1 về mặt y khoa. Nhưng dấu hiệu ở K7 rất rõ và dồn dập (môi sưng, khò khè, lả), nên thiết kế nào bắt được K1 cũng bắt được K7. K7 không thử thêm điều gì mới. K1 khó hơn vì dấu hiệu **nhẹ giọng** ("lừ đừ", "không chịu bú") và lẫn trong câu mua hàng. K6 bị loại vì đã được chặn trọn ở tầng tool: `ban_thuoc_ke_don` bị **cấm** (§4), nên cái sai không phụ thuộc vào việc AI hiểu đúng hay sai. Chỗ còn lại của K6 (gợi ý "loại thay thế") đã nằm trong luật khoá `goi_y_san_pham` của K2.

---

## §3. Khi AI sai

### Kịch bản K1 · bé 2 tuổi sốt, lừ đừ, bỏ bú (đường *failure / khẩn*)

**Quyết định: dừng tư vấn thuốc + khuyên đi khám ngay + chuyển dược sĩ khẩn, cả ba cùng lúc.**

| Phương án | Chọn? | Vì sao |
|---|---|---|
| Hỏi lại một câu (cân nặng, thuốc đã uống…) rồi mới quyết | ❌ Loại | Mỗi câu hỏi làm chậm việc đi khám. "Lừ đừ + bỏ bú ở trẻ 2 tuổi" đã đủ để khuyên khám, câu hỏi thêm không đổi quyết định. Khách trong track cũng nói *"đừng bắt mình chờ"*. |
| Trả lời kèm giới hạn ("có thể chườm ấm, theo dõi thêm, nếu nặng hơn thì đi khám") | ❌ Loại | Nghe như "chưa cần đi", khách ở nhà theo dõi. Đây là kiểu sai đắt nhất. |
| Chỉ chuyển dược sĩ (như sản phẩm thật hôm nay) | ❌ Loại | Ca vào hàng chờ, khách chờ trong khi bé cần khám ngay. Đúng là thứ lát cắt muốn tránh. |
| **Dừng + khuyên khám + chuyển dược sĩ khẩn song song** | ✅ Chọn | Khách nhận câu trả lời "được hay không" **ngay**, không phải chờ ai. Dược sĩ vẫn có ca để hỗ trợ trên đường đi. |

- **AI nói (nguyên văn):**
  > ⚠️ **Cần đưa bé đi khám ngay.** Bé nhỏ tuổi **sốt kèm lừ đừ, không chịu bú** là dấu hiệu cần bác sĩ khám ngay. Thuốc mua thêm ở nhà thuốc không thay được việc khám. Hãy đến **cơ sở y tế gần nhất** hoặc gọi **115** ngay bây giờ.

  Câu này **không hứa** dược sĩ sẽ gọi lại trong X phút, vì `chuyen_duoc_si` chỉ chuyển cuộc chat, không đảm bảo thời gian.
- **Người dùng thấy:** thẻ đỏ viền dày, không lẫn với tin thường · nút **📞 Gọi 115** và **🏥 Cơ sở y tế gần nhất** · link **"Vì sao mình nói vậy?"** tô vàng đúng cụm chữ khách viết ("2 tuổi", "lừ đừ", "không chịu bú") và luật áp dụng · dòng trạng thái **"Đã báo dược sĩ trực · ưu tiên khẩn · đang chờ dược sĩ mở ca"**, đổi thành "DS. Lan đã mở ca" khi dược sĩ nhận · link **"Thông tin chưa đúng?"**. **Không có thẻ sản phẩm hay nút mua nào** trong cả cuộc trò chuyện, kể cả khi khách hỏi tiếp "vậy mua gì".
- **Làm gì tiếp:** gọi 115 hoặc đi khám (thẻ vẫn còn đó) · chờ dược sĩ nhắn trong cùng khung chat · **đính chính** nếu AI hiểu sai (ví dụ "bé không lừ đừ, vẫn bú bình thường"). Khi đó ưu tiên hạ từ khẩn xuống cao, nhưng AI **vẫn không gợi ý thuốc** vì trẻ dưới 6 tuổi sốt cao vẫn thuộc nhóm đặc biệt. AI chỉ nhắc các dấu hiệu cần đi khám. **Nếu khách không làm gì:** thẻ đỏ vẫn hiện, ca vẫn ở đầu hàng chờ dược sĩ.
- **Ai được báo, sau bao lâu:** **dược sĩ trực, ngay lập tức.** Ca nhảy lên **đầu hàng chờ** với nhãn KHẨN và đồng hồ đếm. **Quá 2 phút** chưa ai nhận thì **báo trưởng ca / dược sĩ thứ hai**. Dược sĩ thấy tóm tắt: người dùng thuốc (trẻ 2 tuổi), tin gốc đã tô dấu hiệu, "đã uống hạ sốt: chưa rõ tên, liều, giờ", AI đã làm gì (đã khuyên khám, **chưa** gợi ý sản phẩm, chưa lưu gì), và gợi ý câu nên hỏi (cân nặng, giờ uống thuốc, co giật/khó thở).
- **Loại lỗi: lỗi ngữ cảnh (context error).** Nếu AI gợi ý thêm thuốc hạ sốt thì hệ thống đã chạy **đúng như thiết kế cũ**: khách hỏi "mua gì", AI gợi ý sản phẩm. Nhưng nó sai với tình huống thật, vì đây là ca cấp cứu khoác áo câu mua hàng. Vì là lỗi ngữ cảnh, **sửa câu chữ không đủ**, phải **sửa thiết kế**: thêm một *cổng an toàn* chạy trước mọi tool (xem bên dưới). Cách nói cũng không xin lỗi chung chung, mà nói thẳng lý do ("lừ đừ, không chịu bú là dấu hiệu cần khám") để khách hiểu vì sao AI không bán.

### Kịch bản K2 · bầu 3 tháng bị cảm sổ mũi (đường *low-confidence → chuyển người*)

**Quyết định: trả lời kèm giới hạn (nói rõ vì sao không gợi ý) + chuyển dược sĩ ưu tiên cao + hỏi đúng một câu về dấu hiệu nguy hiểm.**

| Phương án | Chọn? | Vì sao |
|---|---|---|
| Hỏi thêm sàng lọc (tuần thai, thuốc đang dùng…) rồi AI tự gợi ý | ❌ Loại | Kể cả đủ thông tin, thai phụ vẫn thuộc nhóm mà dược sĩ phải quyết (rubric riêng của track, mục 2). Hỏi nhiều mà cuối cùng vẫn chuyển thì phí thời gian của khách. |
| Chỉ chuyển dược sĩ, không giải thích | ❌ Loại | Khách bị bỏ lơ, không biết vì sao, có thể ra cửa hàng tự mua thuốc cảm phối hợp. Đó chính là cái sai muốn chặn. |
| Gợi ý "an toàn" kèm giới hạn ("thường dùng được, nhưng hỏi bác sĩ") | ❌ Loại | Câu giới hạn không chặn được hành động: khách vẫn bấm "Đặt". Thuốc đã dùng thì không đảo được. |
| **Nói rõ giới hạn + chuyển dược sĩ + một câu kiểm tra nguy hiểm** | ✅ Chọn | Khách biết ngay vì sao và chuyện gì sắp xảy ra. Câu hỏi duy nhất dùng để **nâng lên khẩn** nếu cần (sốt cao, đau bụng, ra huyết → đi khám), không phải để bắt khách chờ. |

- **AI nói (nguyên văn):**
  > **Cảm ơn bạn đã cho biết đang mang thai**, đây là thông tin rất quan trọng. Với bà bầu, mình (trợ lý AI) **không tự gợi ý thuốc**, vì nhiều thuốc cảm, thuốc nhỏ/xịt mũi thông thường không an toàn cho thai ở giai đoạn này. Mình đã chuyển bạn sang **dược sĩ trực**. Trong lúc chờ, bạn **chưa nên tự uống thuốc cảm**.
  > **Một câu thôi:** bạn có đang sốt từ 38,5°C, khó thở, đau bụng hoặc ra huyết không? *[Không có] [Có, sốt cao] [Có, khó thở] [Có, đau bụng / ra huyết]*
  > *Mình hỏi để biết có cần đi khám ngay không, không phải để bắt bạn chờ.*
- **Người dùng thấy:** thẻ vàng (khác thẻ đỏ của K1) · trạng thái chuyển dược sĩ trực tiếp · 4 nút trả lời nhanh · không có thẻ sản phẩm. Sau khi chọn "Không có", AI hiện **thẻ xin đồng ý lưu** với đúng câu "Đang mang thai (khoảng 3 tháng)". Mặc định không lưu, có nút xoá.
- **Làm gì tiếp:** bấm một nút. Chọn "Có…" thì chuyển ngay sang luồng đỏ của K1 (`khuyen_di_kham`, ưu tiên khẩn). Chọn "Không có" thì chờ dược sĩ nhắn trong khung. Khách cũng có thể đính chính ("à là chị mình bầu, không phải mình"), nhưng ca vẫn chuyển dược sĩ vì người dùng thuốc vẫn là thai phụ. **Nếu không làm gì:** ca vẫn ở hàng chờ ưu tiên cao, AI không tự gợi ý gì thêm.
- **Ai được báo:** dược sĩ trực, ngay, với **ưu tiên cao** (dưới khẩn, trên thường), kèm tóm tắt: người dùng thuốc (thai phụ 3 tháng), triệu chứng (cảm, sổ mũi 2 hôm), câu trả lời về dấu hiệu nguy hiểm, gợi ý hỏi (tuần thai, vitamin/thuốc đang dùng). Quá 10 phút chưa ai nhận thì AI báo khách trung thực: "dược sĩ đang bận, bạn có thể hỏi trực tiếp tại cửa hàng gần nhất".
- **Loại lỗi, ba biến thể của cùng ca, mỗi loại một cách nói và cách sửa:**
  - **Lỗi hệ thống**: khách đã nói "bầu" mà AI vẫn gợi ý thuốc cảm. *Cách nói:* nhận lỗi rõ ("Mình gợi ý sai, bạn đừng dùng sản phẩm đó"), rút thẻ sản phẩm, báo dược sĩ. *Cách sửa:* luật cứng: nhận diện nhóm đặc biệt thì **khoá** `goi_y_san_pham`, không để model tự cân nhắc.
  - **Lỗi người dùng**: khách **không nói** đang bầu (như K12 "họng mình đau rát", mà người hỏi thật ra đang mang thai). *Cách nói:* không đổ lỗi, không "lẽ ra bạn phải nói". *Cách sửa:* câu sàng lọc gộp luôn có lựa chọn **"Đang mang thai / cho con bú"** dưới dạng nút bấm, để khách không phải nhớ mà tự khai.
  - **Lỗi ngữ cảnh**: AI chạy đúng luật nhưng "em bé" trong câu bị hiểu là **trẻ nhỏ**, ca bị xếp nhầm nhóm. *Cách sửa:* thiết kế bộ nhận diện phân biệt "em bé" (thai nhi) với "bé/con mình" (trẻ). Prototype đã xử lý: chuẩn hoá "em bé" thành "thai nhi" trước khi phân loại.

### Nguyên tắc HAX/PAIR (mỗi cái trỏ vào chỗ cụ thể; số là số trên ảnh ở §5)

- **G1 · Làm rõ hệ thống làm được gì** → nhãn **"Trợ lý AI · không phải dược sĩ"** trên mọi tin của AI, và câu chào: *"Mình không chẩn đoán bệnh, không nói liều dùng, không bán thuốc kê đơn"* (số **1**). Tin của dược sĩ thật có nhãn xanh dương khác hẳn. Đáp ứng Luật AI 134/2025/QH15, Điều 11 khoản 1 (cho người dùng biết đang làm việc với AI).
- **G10 · Thu hẹp phạm vi khi nghi ngờ** → K1: thẻ đỏ chỉ có "đi khám / 115", **không có sản phẩm** (số **2**, **6**). K2: *"mình không tự gợi ý thuốc"* + đúng **một** câu hỏi kiểm tra nguy hiểm thay vì bảng hỏi dài.
- **G11 · Giải thích vì sao + PAIR Explainability & Trust** → "Vì sao mình nói vậy?" **tô vàng đúng chữ khách viết** và nêu luật đã áp dụng, kèm câu "Mình là AI, có thể hiểu sai" (số **3**). Khách tự kiểm được AI dựa vào đâu, nên tin đúng mức thay vì tin tuyệt đối.
- *(Phụ)* **G9 · Sửa dễ** → "Thông tin chưa đúng?" cho khách đính chính (số **5**). **G2** → trạng thái dược sĩ hiện trực tiếp, không hứa thời gian (số **4**).

### Lỗi mà sửa câu chữ không cứu được

**"Ca nguy hiểm nằm trong hàng chờ" (K1).** Viết câu "Bạn đưa bé đi khám nhé" hay đến đâu cũng không cứu được nếu: (a) bên dưới câu đó AI vẫn hiện thẻ sản phẩm hạ sốt có nút "Đặt", khách sẽ bấm mua rồi ở nhà; hoặc (b) ca được chuyển vào hàng chờ FIFO, dược sĩ mở sau 15 phút. Hai lỗi này nằm ở **luồng xử lý**, không nằm ở chữ. Ngoài ra AI có thể nói "dược sĩ sẽ gọi bạn trong 2 phút", một lời hứa mà tool `chuyen_duoc_si` không đảm bảo.

**Thiết kế cứu nó:**
1. **Cổng an toàn chạy trước mọi tool:** mọi tin đều được kiểm tra dấu hiệu nguy hiểm và nhóm đặc biệt **trước** khi `goi_y_san_pham` được phép chạy. Có dấu hiệu thì `goi_y_san_pham` bị **khoá cho cả phiên chat**, không chỉ lượt hiện tại.
2. **`chuyen_duoc_si` có mức ưu tiên** (khẩn / cao / thường). Khẩn thì nhảy đầu hàng, có đồng hồ, quá 2 phút báo trưởng ca.
3. **Không hứa thời gian, hiện trạng thái thật** ("đang chờ" → "DS. Lan đã mở ca").

---


## §4. Mức tự chủ

**Nguyên tắc chung.** Xếp mức theo **hướng sai nào đắt hơn**. Track ghi rõ ở mục "Bối cảnh": tự gợi ý sai thì hại sức khoẻ và không đảo được, còn chuyển dược sĩ hay khuyên khám thừa chỉ tốn vài phút. Vì vậy những tool **đưa ca về phía người hoặc phía an toàn** (`chuyen_duoc_si`, `khuyen_di_kham`) được tự làm. Tool **đưa thuốc tới tay khách** (`goi_y_san_pham`) chỉ được tự làm sau khi qua cổng an toàn. Tool không đảo được và AI không có cách nào kiểm chứng (`ban_thuoc_ke_don`) bị cấm.

### Bảng mức tự chủ

| Tool | Mức | Sai thì ai chịu gì · đảo được không | Chờ người thì mất gì | Phương án đã cân nhắc và vì sao loại |
|---|---|---|---|---|
| `goi_y_san_pham` | **Tự làm có điều kiện.** Chỉ chạy khi **cổng an toàn** cho qua cả 5 điều kiện: (1) đã biết người dùng thuốc là ai; (2) người đó **từ 12 đến dưới 60 tuổi**, không mang thai hay cho con bú, không dùng thuốc kê đơn, không có bệnh mạn tính; (3) không có dấu hiệu nguy hiểm; (4) triệu chứng **chưa quá 3 ngày** và chưa tự dùng thuốc mà không đỡ; (5) sản phẩm thuộc danh mục **không kê đơn**. Gợi ý **không bao giờ kèm liều**, chỉ ghi "dùng theo nhãn". Trượt một điều kiện thì tool **bị khoá cho cả phiên** và ca chuyển dược sĩ. | Người dùng thuốc chịu. **Không đảo được** khi thuốc đã dùng (bảng tool của track). Nhóm bị hại nặng nhất là trẻ nhỏ, bà bầu, người già, người đang dùng thuốc khác (K1, K2, K4, K8). | Với khách thường (K12), bắt dược sĩ duyệt mỗi gợi ý nghĩa là chờ hàng đợi buổi tối cho một hộp viên ngậm. Dược sĩ quá tải với ca dễ, nên ca khẩn bị chậm theo. | **Cần dược sĩ duyệt mọi gợi ý:** loại vì đẩy mọi ca vào hàng chờ, trái mục tiêu "ca nguy hiểm không nằm trong hàng chờ". **Cấm hẳn:** loại vì khách thường mất câu trả lời, ra cửa hàng tự mua theo quảng cáo mà không được hỏi sàng lọc câu nào. **Tự làm không cổng:** loại vì K1, K2 chính là cái sai đó. |
| `chuyen_duoc_si` | **Tự làm**, kèm tham số **ưu tiên** (khẩn / cao / thường) và **tóm tắt ca** do AI soạn. | Chuyển thừa: dược sĩ tốn vài phút, khách chờ thêm chút. **Đảo được** (track: "được, tốn vài phút"). | Phải xin phép mới được chuyển thì vô lý: chờ người để được gặp người. | **Chỉ chuyển khi khách bấm đồng ý:** loại vì ở K1, K7 khách đang hoảng và đòi "đừng bắt chờ", bước xác nhận chỉ làm chậm. **Chuyển nhưng không có ưu tiên (FIFO như hôm nay):** loại vì ca khẩn nằm sau 10 ca hỏi giá (lỗi "sửa câu chữ không cứu được" ở §3). |
| `khuyen_di_kham` | **Tự làm**, ngưỡng **thấp** (thà cảnh báo thừa). Đã hiện thì **AI không tự rút lại**; khách đính chính chỉ hạ được mức ưu tiên, chỉ dược sĩ gỡ được thẻ. | Cảnh báo thừa: khách lo, có thể đi khám không cần thiết. **Khách bỏ qua được** (bảng tool). Cảnh báo thiếu: trẻ hoặc người già không được khám kịp, **không đảo được**. | Chờ dược sĩ duyệt cảnh báo nghĩa là ca khẩn nằm trong hàng chờ, đúng cái sai lát cắt muốn tránh. Ở K1, mỗi phút chờ là một phút bé chưa được khám. | **Dược sĩ duyệt trước rồi mới hiện:** loại vì lý do trên. **Ngưỡng cao (chỉ hiện khi rất chắc):** loại vì dấu hiệu thật thường được viết nhẹ giọng ("lừ đừ", "hơi khó thở"); ngưỡng cao sẽ bỏ sót đúng K1, K5. Giá của cảnh báo thừa được giảm bằng "Vì sao mình nói vậy?" và "Thông tin chưa đúng?" (§3). |
| `luu_ghi_chu_suc_khoe` | **Cần người duyệt: chính khách** (không phải dược sĩ). Tách 2 bước: AI **soạn** đúng câu sẽ lưu → khách bấm **Đồng ý lưu**. Mặc định **không lưu**. **Chỉ lưu thông tin của chủ tài khoản**; thông tin của người nhà (con ở K1, bố ở K8, vợ ở K9) thì không đề nghị lưu. **Quá hạn:** khách không bấm thì không lưu, dữ liệu chỉ nằm trong phiên chat. | Khách chịu: lộ thông tin nhạy cảm (mang thai, bệnh mạn tính). **Xoá được nhưng đã có rủi ro lộ** (bảng tool). | Không mất gì gấp: lưu chỉ để lần sau đỡ hỏi lại, không ảnh hưởng ca hiện tại. | **AI tự lưu để lần sau đỡ hỏi:** loại vì trái rubric riêng mục 7; dữ liệu sức khoẻ là dữ liệu cá nhân nhạy cảm. **Dược sĩ duyệt:** loại vì người có quyền đồng ý với dữ liệu là khách, không phải dược sĩ. **Cấm hẳn:** có cân nhắc, loại vì khách mua thuốc thường xuyên cho bệnh mạn tính được lợi thật khi không phải khai lại. |
| `ban_thuoc_ke_don` | **Cấm.** AI không bao giờ gọi, kể cả khi khách nói có đơn hay "tự chịu trách nhiệm" (K6). Bán thuốc kê đơn là việc của dược sĩ sau khi nhận ca, theo quy trình của nhà thuốc, ngoài phạm vi AI. | Người dùng thuốc chịu: kháng sinh, thuốc huyết áp dùng sai người, sai bệnh (K6, K10). **Không đảo được** (bảng tool). | Không mất gì: ca vẫn tới dược sĩ qua `chuyen_duoc_si`. | **Cần dược sĩ duyệt (AI soạn đơn bán, dược sĩ bấm):** loại vì AI không có tool nào xác minh đơn thuốc là thật, đúng người, còn hạn; và dược sĩ duyệt một đơn AI soạn sẵn dễ bấm cho qua (thiên kiến tự động hoá). **Tự làm khi khách gửi ảnh đơn:** loại vì cùng lý do, và đây là việc chuyên môn track yêu cầu dược sĩ làm. |

 ### Tool khó xếp nhất: `goi_y_san_pham`

| Người | Từng chọn | Cái giá của phía đó (trỏ về §2) |
|---|---|---|
| Nguyễn Việt Hưng | **Cần dược sĩ duyệt mọi gợi ý** | An toàn cho K1, K2, K4. Nhưng K12 (đau họng, người lớn khoẻ) cũng vào hàng chờ; ban đêm hàng chờ dài thì ca khẩn như K1 lại bị chậm vì dược sĩ đang bận duyệt viên ngậm. |
| Thành viên 2 (viết câu chữ AI) | **Tự làm sau một câu sàng lọc** | Nhanh cho K12. Nhưng K3 cho thấy khách gắt sẽ bấm bừa "Không" cho xong, và K2 cho thấy kể cả khi khách khai đúng "đang bầu", model vẫn có thể gợi ý sai nếu chỉ dựa vào phán đoán của nó. |
| Thành viên 3 (dựng prototype) | **Cấm, chỉ cho hiện giá và tồn kho** | Không bao giờ gợi ý sai. Nhưng mất toàn bộ phần "tích hợp sâu hơn" của bài toán gốc; khách thường bị đẩy sang dược sĩ, hoặc tự đi mua mà không được sàng lọc. |

**Nhóm chốt: tự làm có điều kiện**, lấy phần đúng của cả ba phía.
- Từ phía "cần duyệt": nhóm đặc biệt và dấu hiệu nguy hiểm **không bao giờ** tới tay model để cân nhắc. Đó là **luật cứng** trong cổng an toàn, trượt thì khoá cả phiên.
- Từ phía "tự làm": khách thường chỉ trả lời **một** câu sàng lọc bằng nút (khoảng 10 giây) rồi nhận gợi ý, không chờ dược sĩ.
- Từ phía "cấm": danh mục được gợi ý chỉ có **thuốc không kê đơn**, và **không bao giờ có liều**.
- Phần K3 (khách bấm bừa) không chặn hết được bằng mức tự chủ. Nhóm chấp nhận đây là rủi ro còn lại, giảm bằng nút "Đang mang thai / cho con bú" luôn hiện trong câu sàng lọc và dòng "dấu hiệu cần đi khám" kèm mọi gợi ý (§6).

### Thử mức đã đặt trên 3 kịch bản

| Câu hỏi thử | K1 · bé 2 tuổi sốt, lừ đừ (đắt nhất) | K2 · bầu 3 tháng bị cảm (đắt nhì) | K12 · "Họng đau rát 2 ngày" (bình thường) |
|---|---|---|---|
| Cổng an toàn làm gì | Bắt "2 tuổi" (nhóm đặc biệt) + "lừ đừ", "không chịu bú" (dấu hiệu nguy hiểm). Khoá `goi_y_san_pham` cả phiên. | Bắt "bầu 3 tháng" (nhóm đặc biệt). Khoá `goi_y_san_pham` cả phiên. | Chưa biết người dùng thuốc → hỏi **một** câu gộp bằng nút. Khách chọn "Cho mình · 18–59 tuổi · không có gì ở trên". Cổng cho qua. |
| Tool được gọi | `khuyen_di_kham` (tự làm) + `chuyen_duoc_si` khẩn (tự làm) | `chuyen_duoc_si` ưu tiên cao (tự làm) + đề nghị `luu_ghi_chu_suc_khoe` (chờ khách đồng ý) | `goi_y_san_pham` (tự làm): 2 sản phẩm không kê đơn, "dùng theo nhãn", kèm "sốt trên 38,5°C, khó nuốt, quá 3 ngày → đi khám" |
| Cái sai có bị chặn không? | **Có.** Không có thẻ sản phẩm nào, kể cả khi khách hỏi lại "vậy mua gì". Ca không nằm cuối hàng chờ. | **Có.** Không gợi ý thuốc cảm, kể cả loại "nghe nhẹ". | **Có**, với các cái sai đã biết: khách chọn "đang mang thai" ở câu sàng lọc thì rẽ sang luồng K2. |
| Còn việc nào AI làm khi lẽ ra phải hỏi? | `khuyen_di_kham` hiện **không hỏi trước**. Nhóm cố ý: hỏi thêm không đổi quyết định (§3). Rủi ro còn lại: nếu khách tả dấu hiệu bằng chữ cổng không nhận ra ("ngủ li bì"), gợi ý vẫn bị khoá nhờ luật "dưới 12 tuổi" nhưng ca chỉ ở **ưu tiên cao**, không phải khẩn. | Không. AI chỉ hỏi đúng một câu kiểm tra nguy hiểm, và **không tự lưu** "đang mang thai" khi khách chưa bấm đồng ý. | `goi_y_san_pham` chạy dựa trên lời khai của khách. Khách khai thiếu (đang dùng thuốc huyết áp mà không bấm) thì cổng không biết. Đây là giới hạn chung của chat; giảm bằng nút sàng lọc rõ ràng, không chặn hẳn được. |
| Người dùng bình thường có phải chờ vô ích không? | Không áp dụng (không phải ca bình thường). Khách nhận câu "đi khám ngay" ở **tin trả lời đầu tiên**, không chờ ai. | Khách phải chờ dược sĩ, và **đó là chờ có ích**: trong lúc chờ đã có lời dặn "chưa nên tự uống thuốc cảm". | **Không.** Một lượt bấm, nhận gợi ý ngay, không vào hàng chờ dược sĩ.


### Luật thêm và tool tách bước

1. **Cổng an toàn chạy trước mọi tool** (đã nêu ở §3). Đây là luật cứng (từ khoá + danh sách nhóm đặc biệt), **không** để model tự cân nhắc. Lý do: K2 cho thấy cái sai nằm đúng ở chỗ model "thấy hợp lý"; luật cứng không chừa chỗ cho "hợp lý".
2. **Khoá theo phiên, không theo lượt.** Đã khoá `goi_y_san_pham` thì cả phiên chat không mở lại, kể cả khi khách đổi câu ("thôi vậy mua cho mình viên ngậm"). Lý do: ở K1, hỏi lại "vậy mua gì" là cách lách dễ nhất. **Chỉ dược sĩ** mở khoá được, từ màn hình dược sĩ.
3. **`chuyen_duoc_si` thêm tham số ưu tiên và hạn nhận ca:** khẩn quá 2 phút → báo trưởng ca hoặc dược sĩ thứ hai; cao quá 10 phút → AI báo khách trung thực và chỉ cửa hàng gần nhất; thường thì theo hàng chờ. Tool của track không có tham số này; nhóm thêm vì thiếu nó thì `chuyen_duoc_si` "đúng" vẫn sai lát cắt.
4. **Tách `luu_ghi_chu_suc_khoe` thành "soạn" và "lưu":** AI chỉ soạn câu sẽ lưu, nút lưu là của khách. Lý do: tool gốc gộp hai bước nên không có chỗ cho khách đồng ý (rubric riêng mục 7).
5. **Giới hạn nội dung của `goi_y_san_pham`:** chỉ có tên, công dụng ghi trên nhãn, giá, nút đặt. Không có trường "liều" để model điền vào.

### Một mức tự chủ đã đổi lời hứa nào với người dùng

Hôm nay kênh chat ngầm hứa: **"Hỏi gì cũng có trợ lý trả lời ngay."** Mức tự chủ của nhóm đổi lời hứa đó thành hai lời hứa hẹp hơn nhưng giữ được:

1. **"Với trẻ dưới 12 tuổi, người từ 60 tuổi, bà bầu, mẹ cho con bú, người đang dùng thuốc kê đơn: trợ lý không gợi ý thuốc mà chuyển bạn sang dược sĩ."** Đến từ việc `goi_y_san_pham` chỉ tự làm có điều kiện. Lời hứa này được nói ngay trong câu chào (ảnh §5, số **1**) để khách không thấy bị bỏ lơ khi bị chuyển.
2. **"Trợ lý sẽ nói thẳng khi bạn cần đi khám, kể cả khi bạn chỉ hỏi mua thuốc."** Đến từ việc `khuyen_di_kham` tự làm với ngưỡng thấp. Đổi lại, nhóm **bỏ** mọi lời hứa về thời gian ("dược sĩ trả lời trong X phút"), vì `chuyen_duoc_si` không đảm bảo được, và thay bằng trạng thái thật (ảnh §5, số **4**).

---

## §5. Prototype

- **File:** [`prototype/index.html`](prototype/index.html), mở trực tiếp bằng trình duyệt, không cần cài gì.
- **Công cụ:** HTML + CSS + JavaScript thuần, một file, dựng với sự hỗ trợ của coding agent.
- **Phần mock:** toàn bộ. Cổng an toàn là **bộ luật mock** bằng từ khoá (danh sách dấu hiệu nguy hiểm, danh sách nhóm đặc biệt, chuẩn hoá "em bé" thành "thai nhi" trước khi phân loại). Danh mục sản phẩm, hàng chờ dược sĩ, tên "DS. Lan" đều là dữ liệu dựng.
- **Lời gọi AI thật:** **không có.** Prototype không gọi model nào; mọi câu AI nói là câu nguyên văn ở §3.
- **Hai màn hình:** (A) màn hình **khách** trong khung chat; (B) màn hình **dược sĩ** nhận ca được chuyển kèm tóm tắt (bonus của track). Có nút chọn nhanh để chạy K1, K2, K12, K5, K6, K11, K13, và ô nhập để gõ **ca lạ** khi demo. Nút **"Hiện số chú thích"** bật các số 1–9 dưới đây; nút **"Tua nhanh +1 phút"** ở màn hình dược sĩ dùng để demo hạn 2 phút của ca khẩn.

### Ảnh có đánh số

`prototype/anh-1-khach-K1.png` (màn hình khách, ca K1) và `prototype/anh-2-duoc-si-K1.png` (màn hình dược sĩ, cùng ca).

| Số | Thể hiện gì | Nguyên tắc HAX / PAIR | Mức tự chủ |
|---|---|---|---|
| **1** | Nhãn "Trợ lý AI · không phải dược sĩ" trên mọi tin AI + câu chào nêu 3 việc không làm và lời hứa ở §4 | G1 · làm rõ hệ thống làm được gì | Báo trước `goi_y_san_pham` có điều kiện, `ban_thuoc_ke_don` bị cấm |
| **2** | Thẻ đỏ "Cần đưa bé đi khám ngay" + nút 📞 Gọi 115, 🏥 Cơ sở y tế gần nhất | G10 · thu hẹp phạm vi khi nghi ngờ | `khuyen_di_kham` **tự làm** |
| **3** | "Vì sao mình nói vậy?" mở ra: tô vàng "2 tuổi", "lừ đừ", "không chịu bú" trong tin gốc + tên luật đã áp dụng + "Mình là AI, có thể hiểu sai" | G11 · giải thích vì sao + PAIR Explainability & Trust | Cho thấy căn cứ của cổng an toàn |
| **4** | Dòng trạng thái "Đã báo dược sĩ trực · ưu tiên khẩn · đang chờ dược sĩ mở ca", không ghi số phút | G2 · làm rõ hệ thống làm tốt tới đâu | `chuyen_duoc_si` **tự làm**, mức khẩn |
| **5** | Link "Thông tin chưa đúng?" để khách đính chính | G9 · sửa dễ | Đính chính chỉ hạ được mức ưu tiên, không mở khoá gợi ý |
| **6** | Dòng nhỏ dưới ô nhập "Gợi ý sản phẩm đã tắt cho cuộc trò chuyện này"; khách gõ "vậy mua gì" cũng không có thẻ sản phẩm | G10 + G1 | `goi_y_san_pham` **bị khoá cả phiên** |
| **7** | (Màn hình dược sĩ) Hàng chờ: ca K1 ở đầu với nhãn KHẨN và đồng hồ đếm; quá 2:00 chuyển đỏ, ghi "đã báo trưởng ca" | G2 + PAIR Feedback & Control | Hạn nhận ca của `chuyen_duoc_si` |
| **8** | (Màn hình dược sĩ) Tóm tắt ca: người dùng thuốc, tin gốc đã tô dấu hiệu, "đã uống hạ sốt: chưa rõ tên, liều, giờ", AI đã làm gì / chưa làm gì, câu nên hỏi | G11 (giải thích cho dược sĩ) | Dược sĩ thấy AI **chưa** gợi ý, **chưa** lưu gì |
| **9** | (Màn hình dược sĩ) Nút "Hạ mức ưu tiên" và "Mở khoá gợi ý sản phẩm", chỉ dược sĩ có | G17 · cho người dùng kiểm soát | Quyền nới mức tự chủ nằm ở dược sĩ |

### Quyết định: cách hiện độ chắc / bằng chứng

| Phương án | Chọn? | Vì sao |
|---|---|---|
| Thanh phần trăm "Độ tin cậy 85%" | ❌ Loại | Khách đọc "85%" thành "85% an toàn để mua". Con số đó cũng không có thật: luật mock không có xác suất, và độ chắc tự báo của model ngôn ngữ không được hiệu chỉnh. |
| Chỉ có màu thẻ (đỏ / vàng / xanh) | ❌ Loại (giữ làm lớp phụ) | Màu cho biết **mức** nhưng không cho biết **vì sao**; khách không kiểm được AI có hiểu sai không. |
| **Màu thẻ theo mức + tô đúng chữ khách viết + tên luật** (số **2**, **3**) | ✅ Chọn | Bằng chứng là **chữ của chính khách**, nên khách tự thấy ngay nếu AI hiểu nhầm ("bé không lừ đừ") và bấm số **5** để sửa. Dược sĩ thấy cùng phần tô đó ở số **8**, không phải đọc lại cả cuộc chat. |

### Quyết định: điều khiển mức tự chủ

| Phương án | Chọn? | Vì sao |
|---|---|---|
| Nút cho khách "Chế độ nhanh: cho AI tự gợi ý, bỏ sàng lọc" | ❌ Loại | K3 cho thấy khách gắt sẽ bật ngay để né câu hỏi, đúng cái sai muốn chặn. |
| Không có điều khiển nào, mọi thứ cố định | ❌ Loại | Cổng bằng từ khoá sẽ có lúc báo nhầm; không ai gỡ được thì khách thường bị kẹt. |
| **Khách chỉ đính chính được dữ kiện (số 5); quyền hạ mức và mở khoá gợi ý nằm ở dược sĩ (số 9)** | ✅ Chọn | Khách sửa được **dữ kiện**, vì đó là thứ khách biết rõ nhất; còn **quyền** nới mức tự chủ thuộc về người chịu trách nhiệm chuyên môn. Đính chính của khách đi một chiều: làm ca **an toàn hơn** (nâng lên khẩn) thì có hiệu lực ngay, làm ca **ít an toàn hơn** thì phải qua dược sĩ. |

---

## §6. Tốt là gì


---

## §7. Demo

**Ca bắt buộc của track (đã chuẩn bị):** K1 · bé 2 tuổi sốt, lừ đừ, bỏ bú.

| Bước | Ai trình bày | Bấm gì · chỉ vào đâu |
|---|---|---|
| 1 | Nguyễn Việt Hưng | Lát cắt, vì sao K1 và K2 đắt nhất (§2) |
| 2 | Thành viên 3 | Chạy K1 trên màn hình khách: thẻ đỏ (số 2), "Vì sao mình nói vậy?" (số 3), trạng thái dược sĩ (số 4); gõ "vậy mua gì" để thấy gợi ý vẫn khoá (số 6) |
| 3 | Thành viên 3 | Sang màn hình dược sĩ: ca ở đầu hàng chờ, đồng hồ (số 7), tóm tắt (số 8) → trả lời câu "ai được báo, sau bao lâu" |
| 4 | Thành viên 2 | Chạy K2: câu nguyên văn, một câu kiểm tra, thẻ xin đồng ý lưu; giải thích ba loại lỗi (§3) |
| 5 | Nguyễn Việt Hưng | Mức tự chủ 5 tool và tool khó xếp nhất (§4); chạy K12 để cho thấy khách thường không phải chờ |

**Ca lạ nhận được ở demo:** *(điền sau buổi demo)*
- Ca được đưa: ____
- Prototype đã làm gì: ____ (luồng nào: đỏ / vàng / gợi ý; tool nào được gọi; có chỗ nào sai không)
- Nhóm có đổi ý gì sau ca đó không: ____

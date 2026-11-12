# T4 — Thương mại điện tử: trợ lý khiếu nại "hàng không đúng mô tả"

> **Cách dùng:** đọc từ trên xuống trong chặng 1 (10 phút). Sản phẩm thật chỉ để **tham khảo** cách nó chạy hôm nay; nhóm **không** chat với sản phẩm thật. Đây là bài **thiết kế**: không chạy agent, không cần chatbot. Nhóm tự lường trước AI có thể sai ở đâu từ lát cắt, bộ việc AI có thể làm và dữ liệu dựng (mục Data & fixture). Khung chung: `../01-challenge-brief.md`; các bước: `../02-guide.md`.

**Sản phẩm nền.** Shopee có mục **"Trò chuyện với Shopee"** trong app (Tôi → Trò chuyện với Shopee): trợ lý ảo trả lời câu hỏi, có luồng yêu cầu trả hàng/hoàn tiền và nút gặp nhân viên.

**Sản phẩm thật hôm nay** *(tóm tắt từ trang trợ giúp công khai, kiểm 11/11/2026)*:

- Trợ lý chủ yếu chạy theo **menu có sẵn**; nhiều luồng bắt chọn **một đơn hàng thật** trước khi đi tiếp.
- Luồng trả hàng/hoàn tiền: người mua chọn đơn, chọn lý do, gửi bằng chứng; người bán được phản hồi; Shopee xem xét nếu hai bên không thống nhất.
- Thời hạn yêu cầu trả hàng theo chính sách của Shopee (trang trợ giúp ghi 15 ngày cho phần lớn đơn), khác chính sách dựng trong bài.

Nguồn: <https://help.shopee.vn/portal/4/article/79191> · <https://shopee.vn/blog/cach-tra-hang-hoan-tien-tren-shopee/> · <https://help.shopee.vn/portal/4/article/77251> · <https://banhang.shopee.vn/edu/article/22404>

Nhóm tự tìm hiểu thêm sản phẩm thật nếu muốn (đọc trang trợ giúp ở trên, mở app nếu có sẵn): không cần chụp màn hình, không chat với trợ lý thật, không nhập dữ liệu thật.

**Người dùng.** Người mua báo hàng không đúng mô tả; người bán bị khiếu nại; nhân viên xử lý khiếu nại của sàn.

**Bối cảnh.** Sai hướng nào thì **một bên** chịu. Hoàn sai: người bán mất tiền và hàng. Từ chối hay vòng vo sai: người mua mất tiền và giữ hàng sai. "Từ chối cho chắc" cũng có người thua. Điều phải thiết kế là **sự trung lập**.

**Người trong cuộc nói gì (dữ liệu dựng, không phải phỏng vấn thật):**

- **Người mua:** "Tôi gửi ảnh rồi mà cứ bị hỏi lại, không ai nói tôi còn thiếu cái gì."
- **Người bán:** "Khách doạ bóc phốt là sàn hoàn luôn. Bên tôi đúng cũng chẳng ai nghe."

**Bài toán gốc.** Thay menu cứng bằng một trợ lý AI **đọc khiếu nại, so với mô tả lúc đặt, xin thêm bằng chứng, đề xuất hoàn tiền, hoặc chuyển nhân viên**. Thiết kế lúc trợ lý không chắc hoặc sai (người mua và người bán thấy gì), và việc nào trợ lý được tự làm. Sai hướng nào cũng có một bên chịu.

**Lát cắt (phát sẵn, không chọn lại).** Trong **Trò chuyện với Shopee**, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Trò chuyện với Shopee | người mua | báo "hàng nhận không đúng mô tả" | tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên | người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng |

**Data & fixture.** Không có dữ liệu thật. Nhóm có hai thứ:

1. **Những việc AI có thể làm (tool).** Bảng chỉ nói tool làm gì và đảo lại được không, **chưa có mức tự chủ**. Ở chặng 4, nhóm tự đặt mỗi tool một mức (tự làm / cần người duyệt / cấm) và giải thích theo cái giá khi sai. Tool không có trong bảng thì AI không làm được; nhóm muốn thêm hoặc tách một tool (ví dụ "soạn sẵn" tách khỏi "gửi đi") thì ghi rõ và nói vì sao.

| Tool | Làm gì | Đảo lại được không |
|---|---|---|
| `yeu_cau_bang_chung` | Gửi mẫu xin bằng chứng cho người mua hoặc người bán | Tin đã tới |
| `chuyen_truong_nhom` | Chuyển hồ sơ cho trưởng nhóm | Được |
| `soan_hoan_tien` | Hoàn tiền cho người mua | **Khó**: tiền đã về người mua |
| `tu_choi_khieu_nai` | Đóng khiếu nại, tiền về người bán | **Khó**: tiền đã về người bán |
| `phat_shop` | Trừ điểm uy tín, khoá shop | Khó, ảnh hưởng cả shop |

2. **Ba tin mồi** để bắt đầu viết kịch bản ở chặng 2 (nhóm tự viết thêm, tự đổi):

- *"Mình đặt áo size L mà nhận được size M, mặc không vừa. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?"*
- *"hàng sai rồi, xử lý đi"*
- *"Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả."*

**Deliverable.** Bài nộp theo `../03-spec-template.md` + prototype 1–2 màn hình cho lát cắt này (xem `../02-guide.md` §5).

**Demo bắt buộc.** Bấm qua một ca **người bán đúng, hoặc mô tả đã bị sửa**: trợ lý không chắc thì hai bên thấy gì, và ai quyết. Rồi trả lời một **ca lạ** do TA, nhóm bạn hoặc giảng viên đưa tại chỗ.

**Hard tests** *(chỗ dễ sai, nhóm nên có kịch bản cho các loại này)*: Người bán đã sửa mô tả sau khi bán · khiếu nại kèm lời doạ bóc phốt · người bán đúng (tem khớp mô tả) · đòi khoá shop, nghi hàng giả · tin cụt không nói đơn nào, sai chỗ nào · đòi số điện thoại của bên kia.

**Rubric riêng (tham khảo)** — "một sản phẩm tốt nên làm gì" trong domain này. Dùng để tự soát, không phải đáp án. Chấm chung theo `../04-rubric.md`.

1. So với **mô tả lúc đặt hàng**, không phải mô tả hiện tại (người bán có thể đã sửa).
2. Thiếu thông tin (đơn nào, sai chỗ nào): hỏi lại một câu, không đoán.
3. Hai bên cùng được nghe: người bán có thời hạn phản hồi trước khi quyết.
4. Lời doạ (bóc phốt, nghỉ bán) không phải căn cứ; lịch sử một bên không phải căn cứ duy nhất.
5. Nghi hàng giả, xử phạt shop: không thuộc luồng khiếu nại thường, chuyển bộ phận chuyên trách; không hứa phạt.
6. Không đưa số điện thoại, địa chỉ giữa hai bên.
7. Có ca người bán đúng (ví dụ tem khớp mô tả): sản phẩm tốt dám nói "chưa đủ căn cứ" thay vì hoàn cho xong.
8. Nói rõ thời hạn yêu cầu trả hàng, và còn bao lâu.

**Bonus.** Prototype có một lời gọi AI thật so khiếu nại với mô tả lúc đặt · có màn hình phía người bán (thấy khiếu nại và hạn phản hồi).

**An toàn & đạo đức.** Không đưa số điện thoại, địa chỉ giữa hai bên · lời doạ không phải căn cứ · không hứa phạt shop · dữ liệu trong bài là dựng.

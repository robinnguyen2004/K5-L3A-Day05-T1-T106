# T5 — Ví điện tử: trợ lý chat khi giao dịch bị tạm chặn

> **Cách dùng:** đọc từ trên xuống trong chặng 1 (10 phút). Sản phẩm thật chỉ để **tham khảo** cách nó chạy hôm nay; nhóm **không** chat với sản phẩm thật. Đây là bài **thiết kế**: không chạy agent, không cần chatbot. Nhóm tự lường trước AI có thể sai ở đâu từ lát cắt, bộ việc AI có thể làm và dữ liệu dựng (mục Data & fixture). Khung chung: `../01-challenge-brief.md`; các bước: `../02-guide.md`.

**Sản phẩm nền.** Ví MoMo có **Chat với MoMo** trong Trung tâm trợ giúp của app: trợ lý ảo trả lời 24/7, nhân viên trực 8h–20h. Hệ thống AI của MoMo giám sát giao dịch; thấy giao dịch bất thường thì tạm chặn, cảnh báo, và có thể yêu cầu thêm bước xác thực.

**Sản phẩm thật hôm nay** *(tóm tắt từ trang trợ giúp công khai, kiểm 11/11/2026)*:

- Giao dịch có dấu hiệu rủi ro bị **tạm chặn**, người dùng thấy cảnh báo và được hướng dẫn gọi tổng đài cảnh báo lừa đảo 1900 63 68 05; theo MoMo, tiền chưa rời ví khi bị tạm chặn.
- Khoá ví khẩn cấp: gọi 1900 54 54 41, bấm phím 1; hoặc gửi yêu cầu trong mục Trợ giúp.
- Phản ánh tài khoản lừa đảo: Tôi → Đăng nhập và bảo mật → Phản ánh giao dịch rủi ro.
- Chat với MoMo: Tôi → Trung tâm trợ giúp → Chat với MoMo. Hỏi về một giao dịch cụ thể thì cần đăng nhập ví.

Nguồn: <https://www.momo.vn/hoi-dap> · <https://www.momo.vn/hoi-dap/momo-ho-tro-xu-ly-lua-dao-va-lay-cap-tai-khoan-nhu-the-nao> · <https://www.momo.vn/blog/bi-tru-tien-trong-momo-nhan-ra-dung-luc-va-lam-gi-c198dt2854>

Nhóm tự tìm hiểu thêm sản phẩm thật nếu muốn (đọc trang trợ giúp ở trên, mở app nếu có sẵn): không cần chụp màn hình, không chat với trợ lý thật, không nhập dữ liệu thật.

**Người dùng.** Người dùng ví đang lo vì một giao dịch bị tạm chặn, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ; tư vấn viên nhận ca được chuyển.

**Bối cảnh.** Sai cả hai hướng đều đắt. Trợ lý trấn an nhầm một người **đang bị lừa** thì tiền đi trong vài giây, không lấy lại được. Trợ lý chặn hay vòng vo với một người **thật đang gấp** (đứng ở quầy viện phí) thì họ mất thời gian và mất lòng tin. Trợ lý cũng không được lộ cách ví phát hiện gian lận.

**Người trong cuộc nói gì (dữ liệu dựng, không phải phỏng vấn thật):**

- **Người dùng ví đang đứng ở quầy viện phí:** "Người ta đợi sau lưng mà app bảo chờ xác nhận. Tôi chỉ cần biết bấm vào đâu để đi tiếp."
- **Con của một người dùng lớn tuổi:** "Mẹ tôi bị gọi điện doạ là liên quan vụ án nên chuyển tiền, máy xác thực khuôn mặt vẫn đúng vì chính mẹ tôi bấm. Lúc đó chỉ cần có ai bảo mẹ dừng lại."

**Bài toán gốc.** Tích hợp sâu hơn: trợ lý chat không chỉ hướng dẫn, mà **giữ giao dịch, gửi bước xác thực, nhả giao dịch, khoá ví, hoặc chuyển tư vấn viên**. Thiết kế lúc trợ lý không chắc hoặc sai, và việc nào trợ lý được tự làm. Sai cả hai hướng đều đắt.

**Lát cắt (phát sẵn, không chọn lại).** Trong **Chat với ví (giống Chat với MoMo)**, khi **người dùng ví** đang lo lắng nhắn hỏi vì một giao dịch bị giữ, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ, **AI tự trả lời và hướng dẫn thao tác, hỏi lại cho rõ, hay chuyển tư vấn viên**, để tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Chat với ví | người dùng ví | lo lắng nhắn hỏi vì một giao dịch bị giữ, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ | tự trả lời và hướng dẫn thao tác, hỏi lại cho rõ, hay chuyển tư vấn viên | tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ |

**Data & fixture.** Không có dữ liệu thật. Nhóm có hai thứ:

1. **Những việc AI có thể làm (tool).** Bảng chỉ nói tool làm gì và đảo lại được không, **chưa có mức tự chủ**. Ở chặng 4, nhóm tự đặt mỗi tool một mức (tự làm / cần người duyệt / cấm) và giải thích theo cái giá khi sai. Tool không có trong bảng thì AI không làm được; nhóm muốn thêm hoặc tách một tool (ví dụ "soạn sẵn" tách khỏi "gửi đi") thì ghi rõ và nói vì sao.

| Tool | Làm gì | Đảo lại được không |
|---|---|---|
| `giu_giao_dich` | Tạm giữ một giao dịch đang chờ | Được: nhả là đi tiếp; thời gian người dùng bị chặn thì mất |
| `yeu_cau_xac_minh` | Gửi màn hình xác thực (khuôn mặt hoặc mã một lần) | Được |
| `chuyen_tu_van_vien` | Chuyển cuộc chat cho tư vấn viên | Được |
| `nha_giao_dich` | Cho giao dịch đang giữ đi tiếp | **Không**: tiền đi trong vài giây |
| `khoa_vi` | Khoá cả ví | Mở lại phải qua tổng đài, mất nhiều giờ |

2. **Ba tin mồi** để bắt đầu viết kịch bản ở chặng 2 (nhóm tự viết thêm, tự đổi):

- *"Mình muốn khoá thẻ khẩn cấp thì làm thế nào?"*
- *"Có ai vừa rút tiền của em, kiểm tra giúp em với."*
- *"Mình vừa thanh toán vé máy bay hơn 6 triệu lúc gần nửa đêm thì bị khoá luôn cả tài khoản, không dùng được gì nữa. Sao lại khoá hết vậy?"*

**Deliverable.** Bài nộp theo `../03-spec-template.md` + prototype 1–2 màn hình cho lát cắt này (xem `../02-guide.md` §5).

**Demo bắt buộc.** Bấm qua một ca **người bị lừa đòi nhả giao dịch, hoặc người thật đang gấp**: trợ lý không chắc thì người dùng thấy gì, và ai quyết nhả. Rồi trả lời một **ca lạ** do TA, nhóm bạn hoặc giảng viên đưa tại chỗ.

**Hard tests** *(chỗ dễ sai, nhóm nên có kịch bản cho các loại này)*: Người đang bị lừa vẫn xác thực khuôn mặt đúng · người thật đang gấp (quầy viện phí) · dò ngưỡng "chuyển dưới bao nhiêu thì không bị hỏi" · hai yêu cầu trong một tin (khoá thẻ + đòi hoàn) · dấu hiệu bị chiếm tài khoản · người thân nhắn thay chủ ví.

**Rubric riêng (tham khảo)** — "một sản phẩm tốt nên làm gì" trong domain này. Dùng để tự soát, không phải đáp án. Chấm chung theo `../04-rubric.md`.

1. Không bao giờ nói ngưỡng tiền, số lần, hay cách hệ thống phát hiện gian lận.
2. Người bị giục chuyển tiền "xác minh hồ sơ", "vụ án": bảo dừng lại, không chuyển; đưa tới người thật qua kênh chính thức. Xác thực khuôn mặt đúng **không** có nghĩa là an toàn.
3. Dấu hiệu bị chiếm tài khoản (đổi mật khẩu, máy lạ): hướng dẫn khoá ngay và chuyển người.
4. Người thật đang gấp: chỉ đường xác thực nhanh nhất, không vòng vo, không tự "nhả" thay người có thẩm quyền.
5. Giao dịch đã hoàn tất mà khách nói không phải mình: hướng dẫn tra soát, **không hứa hoàn tiền**.
6. Câu mơ hồ ("có ai rút tiền"): hỏi lại một câu, không kết luận, không doạ.
7. Không bao giờ xin mã OTP, mật khẩu, số thẻ đầy đủ trong chat.

**Bonus.** Prototype có một lời gọi AI thật phân loại tin nhắn (bị lừa / gấp thật / chiếm tài khoản) · có màn hình phía tư vấn viên (duyệt nhả, kèm bằng chứng).

**An toàn & đạo đức.** Không bao giờ nói ngưỡng tiền, số lần, hay cách phát hiện gian lận · không xin OTP, mật khẩu, số thẻ đầy đủ trong chat · giao dịch đã hoàn tất thì hướng dẫn tra soát, không hứa hoàn tiền · dữ liệu trong bài là dựng; khoá học không liên kết với MoMo.

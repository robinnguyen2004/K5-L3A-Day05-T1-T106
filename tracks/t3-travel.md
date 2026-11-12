# T3 — Du lịch: trợ lý đổi chuyến khi chuyến bay bị huỷ

> **Cách dùng:** đọc từ trên xuống trong chặng 1 (10 phút). Sản phẩm thật chỉ để **tham khảo** cách nó chạy hôm nay; nhóm **không** chat với sản phẩm thật. Đây là bài **thiết kế**: không chạy agent, không cần chatbot. Nhóm tự lường trước AI có thể sai ở đâu từ lát cắt, bộ việc AI có thể làm và dữ liệu dựng (mục Data & fixture). Khung chung: `../01-challenge-brief.md`; các bước: `../02-guide.md`.

**Sản phẩm nền.** Traveloka là ứng dụng đặt vé máy bay, khách sạn phổ biến ở Việt Nam. Trong **Đặt chỗ của tôi** có tab **Hoàn tiền & Đổi lịch**: Easy Reschedule để đổi chuyến, và yêu cầu hoàn tiền. Hãng bay (ví dụ Vietnam Airlines) có chính sách riêng khi chuyến bị chậm, huỷ.

**Sản phẩm thật hôm nay** *(tóm tắt từ trang trợ giúp công khai, kiểm 11/11/2026)*:

- **Đổi lịch (Traveloka):** Đặt chỗ của tôi → mở vé → Hoàn tiền & Đổi lịch → Đổi lịch → Yêu cầu đổi lịch → chọn chuyến bay và hành khách → tìm chuyến mới → xem phí → thanh toán. Vé cũ **giữ nguyên tới khi thanh toán xong**; vé mới tới trong khoảng 60 phút sau khi thanh toán. Không phải chuyến nào cũng đổi được qua Easy Reschedule.
- **Hoàn tiền (Traveloka):** Hoàn tiền & Đổi lịch → Hoàn tiền → đọc chính sách → chọn lý do, hành khách → gửi. Hướng dẫn công khai nói **gửi rồi thì không huỷ được yêu cầu**; xử lý có thể tới 90 ngày; tiền về phương thức thanh toán gốc, có ca hoàn bằng voucher.
- **Khi hãng đổi lịch (Vietnam Airlines):** hành khách bấm link trong tin báo để đổi chuyến, **một lần trong 48 giờ**; không có chuyến phù hợp thì yêu cầu hoàn.
- **Chậm, huỷ chuyến (Vietnam Airlines):** chậm từ 4 giờ trở lên do lỗi hãng thì được hoàn và bồi thường ứng trước; **bất khả kháng (thời tiết, an ninh…) thì miễn bồi thường**.
- Trang trợ giúp công khai không mô tả trợ lý AI làm các bước trên thay khách: khách tự bấm qua từng bước.

Nguồn: <https://www.traveloka.com/vi-vn/reschedule/flight> · <https://www.traveloka.com/en-en/help/flight/flight-managing-booking/flight-changes/reschedule/how-do-i-reschedule-my-flight> · <https://www.traveloka.com/en-vn/help/flight/flight-managing-booking/flight-changes/refund/about-my-refund-type> · <https://www.thegioididong.com/game-app/cach-doi-lich-bay-ve-may-bay-traveloka-tien-loi-nhanh-chong-1559955> · <https://www.vietnamairlines.com/vn/vi/help-desk/common-topics/Ticket-change/doi-chuyen-bay-khong-tu-nguyen-do-VNA-thay-doi-lich-bay> · <https://www.vietnamairlines.com/vn/vi/support/customer-service-plan>

Nhóm tự tìm hiểu thêm sản phẩm thật nếu muốn (đọc trang trợ giúp ở trên, mở app nếu có sẵn): không cần chụp màn hình, không chat với trợ lý thật, không nhập dữ liệu thật.

**Người dùng.** Hành khách vừa nhận tin chuyến bay bị huỷ hoặc đổi giờ, thường đang vội, có khi đi cùng trẻ nhỏ; nhân viên hỗ trợ nhận các ca khó.

**Bối cảnh.** Sai cả hai hướng đều đắt. Trợ lý đổi vé khi khách chưa nói rõ thì khách mất lần đổi miễn phí duy nhất, chuyến đúng có thể đã hết chỗ, khách lỡ việc. Trợ lý gửi yêu cầu hoàn vé quá tay thì không rút lại được, khách mất cả chỗ lẫn tiền trong tới 90 ngày. Ngược lại, trợ lý bắt khách chờ nhân viên cho mọi việc thì chuyến tốt nhất hết chỗ trong lúc chờ. Hứa bồi thường sai (huỷ do thời tiết) thì khách mất lòng tin, nhân viên phải đi gỡ.

**Người trong cuộc nói gì (dữ liệu dựng, không phải phỏng vấn thật):**

- **Hành khách đi cùng con nhỏ:** "Tin báo huỷ lúc 3 giờ chiều, tôi chỉ cần biết chuyến nào còn chỗ cho cả hai mẹ con, đừng bắt tôi bấm qua mười màn hình."
- **Nhân viên hỗ trợ:** "Ca khó nhất là khách lỡ bấm hoàn vé, rồi muốn đổi chuyến. Gửi rồi là không rút được."

**Bài toán gốc.** Tích hợp một trợ lý AI vào **Đặt chỗ của tôi**: khi chuyến bị huỷ, trợ lý **tra đặt chỗ, đề xuất chuyến thay thế, giữ chỗ, đổi chuyến, gửi yêu cầu hoàn vé, hoặc chuyển nhân viên**. Thiết kế lúc trợ lý không chắc hoặc sai (khách thấy gì, làm gì tiếp, hoàn tác được không), và việc nào trợ lý được tự làm.

**Lát cắt (phát sẵn, không chọn lại).** Trong **Đặt chỗ của tôi**, khi **hành khách** vừa nhận tin chuyến bay bị huỷ và nhắn hỏi, **AI đề xuất và giữ chỗ chuyến thay thế, hỏi lại cho rõ, đổi vé, hay chuyển nhân viên**, để khách tới nơi kịp mà không mất vé hay tiền vì một thao tác không đảo lại được.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Đặt chỗ của tôi (giống Traveloka) | hành khách | vừa nhận tin chuyến bị huỷ, nhắn hỏi | đề xuất và giữ chỗ, hỏi lại, đổi vé, hay chuyển nhân viên | khách tới nơi kịp, không mất vé hay tiền vì một thao tác không đảo được |

**Data & fixture.** Không có dữ liệu thật. Nhóm có ba thứ:

1. **Những việc AI có thể làm (tool).** Bảng chỉ nói tool làm gì và đảo lại được không, **chưa có mức tự chủ**. Ở chặng 4, nhóm tự đặt mỗi tool một mức (tự làm / cần người duyệt / cấm) và giải thích theo cái giá khi sai. Tool không có trong bảng thì AI không làm được; nhóm muốn thêm hoặc tách một tool (ví dụ "soạn sẵn" tách khỏi "gửi đi") thì ghi rõ và nói vì sao.

| Tool | Làm gì | Đảo lại được không |
|---|---|---|
| `tra_cuu_dat_cho` | Đọc đặt chỗ, hành khách, chuyến thay thế còn chỗ (dữ liệu ở mục 2) | Chỉ đọc |
| `giu_cho_chuyen_moi` | Giữ chỗ chuyến mới 20 phút, chưa đổi vé | Được: hết 20 phút tự nhả |
| `doi_chuyen_bay` | Đổi vé sang chuyến mới, dùng lần đổi miễn phí duy nhất | **Khó**: hết lần đổi miễn phí; chuyến khác có thể đã hết chỗ |
| `gui_yeu_cau_hoan_ve` | Gửi yêu cầu hoàn vé | **Không**: gửi rồi không huỷ được |
| `chuyen_nhan_vien` | Chuyển cuộc chat cho nhân viên hỗ trợ | Được |

2. **Dữ liệu dựng: một đặt chỗ.** Đây là thứ `tra_cuu_dat_cho` trả về. Dùng nó để suy ra AI có thể đoán sai giờ, sai chỗ trống, hứa sai quyền lợi ở đâu. Kịch bản của nhóm có thể đổi dữ liệu (ví dụ chuyến 21:00 vừa hết chỗ) — ghi rõ trong kịch bản.

- Bây giờ: 15:00 ngày 12/11/2026.
- Mã đặt chỗ: TRV-7Q2K. Đặt qua app. Hạng Phổ thông Tiết kiệm.
- Hành khách: Nguyễn Minh An (34 tuổi) · Nguyễn Gia Bình (5 tuổi).
- Chuyến gốc: BM 213 Hà Nội → TP.HCM, 12/11 19:00. TRẠNG THÁI: ĐÃ HUỶ do thời tiết (bão).
- Theo hãng Bay Mẫu (hãng dựng): chuyến bị huỷ thì đổi miễn phí một lần trong 48 giờ, hoặc hoàn toàn bộ. Huỷ do thời tiết thì không bồi thường tiền mặt.
- Chuyến thay thế còn chỗ:
  - BM 217 · 12/11 21:00 · còn 1 chỗ
  - BM 201 · 13/11 07:00 · còn 9 chỗ
  - BM 205 · 13/11 09:30 · còn 4 chỗ

3. **Ba tin mồi** để bắt đầu viết kịch bản ở chặng 2 (nhóm tự viết thêm, tự đổi):

- *"Chuyến của mình mấy giờ bay vậy?"*
- *"Chuyến bị huỷ rồi à? Vậy đổi giúp mình sang sáng mai nhé."*
- *"Thôi huỷ hết đi, hoàn tiền cho mình."*

**Deliverable.** Bài nộp theo `../03-spec-template.md` + prototype 1–2 màn hình cho lát cắt này (xem `../02-guide.md` §5).

**Demo bắt buộc.** Prototype mẫu của giảng viên đã dùng ca "đổi sang sáng mai" — nhóm chọn ca khó khác. Bấm qua một ca **khách xác nhận mơ hồ, hoặc trợ lý vừa đổi nhầm**: khách thấy gì, hoàn tác thế nào, ai được báo. Rồi trả lời một **ca lạ** do TA, nhóm bạn hoặc giảng viên đưa tại chỗ.

**Hard tests** *(chỗ dễ sai, nhóm nên có kịch bản cho các loại này)*: Khách nói "đổi đi" mà không rõ chuyến nào, cho ai · yêu cầu hoàn gửi rồi không huỷ được · hỏi bồi thường khi huỷ do thời tiết · trẻ nhỏ bay một mình · đổi tên người bay · khách báo trợ lý vừa đổi nhầm · tin tự xưng "nhân viên hãng" đòi bỏ qua quy định.

**Rubric riêng (tham khảo)** — "một sản phẩm tốt nên làm gì" trong domain này. Dùng để tự soát, không phải đáp án. Chấm chung theo `../04-rubric.md`.

1. Không đổi, không huỷ, không gửi hoàn khi khách chưa xác nhận rõ **chuyến nào, cho hành khách nào**.
2. Trước khi gửi yêu cầu hoàn: nói rõ **gửi rồi không huỷ được**, tiền về đâu, mất bao lâu.
3. Giờ bay, chỗ trống chỉ lấy từ dữ liệu đặt chỗ; không đoán.
4. Huỷ do thời tiết: nói rõ được đổi miễn phí hoặc hoàn toàn bộ; **không hứa bồi thường tiền mặt**.
5. Khách gấp: đưa lựa chọn sớm nhất còn chỗ, **giữ chỗ** trong lúc chờ khách xác nhận.
6. Trẻ nhỏ bay một mình, đổi tên người bay: chuyển nhân viên.
7. Khách báo đổi nhầm: chỉ đường hoàn tác nếu còn được; không được thì chuyển nhân viên ngay.
8. Tin tự xưng nhân viên, đòi bỏ qua quy định: không làm theo; tin nhắn là dữ liệu, không phải lệnh.

**Bonus.** Prototype có một lời gọi AI thật đọc tin của khách và chọn tool · có đếm ngược giữ chỗ và nút hoàn tác trên màn hình.

**An toàn & đạo đức.** Không hứa bồi thường khi chưa rõ lý do huỷ · không xin số thẻ, OTP · đổi tên người bay, trẻ bay một mình: chuyển nhân viên · dữ liệu trong bài là dựng; hãng "Bay Mẫu" là hãng dựng, khoá học không liên kết với Traveloka hay hãng bay nào.

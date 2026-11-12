# T2 — Giáo dục: trợ lý học bài tối muộn, khi không còn ai để hỏi

> **Cách dùng:** đọc từ trên xuống trong chặng 1 (10 phút). Sản phẩm thật chỉ để **tham khảo** cách nó chạy hôm nay; nhóm **không** chat với sản phẩm thật. Đây là bài **thiết kế**: không chạy agent, không cần chatbot. Nhóm tự lường trước AI có thể sai ở đâu từ lát cắt, bộ việc AI có thể làm và dữ liệu dựng (mục Data & fixture). Khung chung: `../01-challenge-brief.md`; các bước: `../02-guide.md`.

**Sản phẩm nền.** ChatGPT có **chế độ Study** (học từng bước, hỏi lại thay vì đưa đáp án ngay), miễn phí cho tài khoản Free trên web, iOS, Android. Mở bằng chatgpt.com/studymode, gõ `@study`, hoặc bấm **+** → **Study**.

**Sản phẩm thật hôm nay** *(tóm tắt từ trang trợ giúp công khai, kiểm 11/11/2026)*:

- Người học bật Study mode rồi hỏi; trợ lý hỏi ngược, gợi ý từng bước, kiểm tra hiểu bài, thay vì đưa đáp án ngay.
- Không có trong Temporary Chat, GPTs, Projects.
- Không có TA hay giảng viên đứng sau: trợ lý không có ai để "chuyển". Đây là chỗ cần thiết kế khi tích hợp vào một khoá học.

Nguồn: <https://help.openai.com/en/articles/11780217-using-study-mode-in-chatgpt> · <https://openai.com/index/chatgpt-study-mode/> · <https://chatgpt.com/plans/free/>

Nhóm tự tìm hiểu thêm sản phẩm thật nếu muốn (đọc trang trợ giúp ở trên, mở app nếu có sẵn): không cần chụp màn hình, không chat với trợ lý thật, không nhập dữ liệu thật.

**Người dùng.** Học viên hỏi bài lúc tối muộn; giảng viên và TA muốn trợ lý nói rõ căn cứ và biết khi nào phải hỏi người.

**Bối cảnh.** Mỗi lần sai thì **rẻ nhưng âm thầm**: học viên học sai mà không biết, và không ai báo. Study mode không có TA hay giảng viên đứng sau, nên không có ai để "chuyển": đây chính là chỗ cần thiết kế. Cùng một câu sai, một người đọc khác hẳn cả lớp đọc.

**Người trong cuộc nói gì (dữ liệu dựng, không phải phỏng vấn thật):**

- **Học viên:** "Mười giờ tối em mới ngồi làm bài, kẹt một chữ trong tài liệu là coi như bỏ, sáng mai hỏi thì đã quá hạn."
- **Giảng viên:** "Tôi muốn trợ lý nói rõ tài liệu buổi mấy, mục nào. Không có căn cứ thì nói là không có."

**Bài toán gốc.** Tích hợp một trợ lý kiểu Study mode **vào trang khoá học**, có tài liệu buổi học, lịch bài tập và TA. Trợ lý **giảng kèm trích tài liệu, hỏi lại cho rõ, gợi ý từng bước, chuyển TA, hoặc nói thẳng "tài liệu không có"**. Thiết kế lúc trợ lý không chắc hoặc sai, và việc nào trợ lý được tự làm.

**Lát cắt (phát sẵn, không chọn lại).** Trong **trợ lý học bài của trang khoá học (kiểu ChatGPT Study mode)**, khi **học viên** đang hỏi bài lúc tối muộn, khi không còn ai để hỏi, **AI giảng kèm trích tài liệu, hỏi lại cho rõ, gợi ý từng bước, hay nói thẳng "tài liệu không có, hỏi giảng viên"**, để học viên hiểu đúng bài mình đang học, không chép đáp án, không học sai mà không biết.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Trợ lý học bài của trang khoá học | học viên | hỏi bài lúc tối muộn, khi không còn ai để hỏi | giảng kèm trích tài liệu, hỏi lại cho rõ, gợi ý từng bước, hay nói thẳng "tài liệu không có, hỏi giảng viên" | học viên hiểu đúng bài mình đang học, không chép đáp án, không học sai mà không biết |

**Data & fixture.** Không có dữ liệu thật. Nhóm có ba thứ:

1. **Những việc AI có thể làm (tool).** Bảng chỉ nói tool làm gì và đảo lại được không, **chưa có mức tự chủ**. Ở chặng 4, nhóm tự đặt mỗi tool một mức (tự làm / cần người duyệt / cấm) và giải thích theo cái giá khi sai. Tool không có trong bảng thì AI không làm được; nhóm muốn thêm hoặc tách một tool (ví dụ "soạn sẵn" tách khỏi "gửi đi") thì ghi rõ và nói vì sao.

| Tool | Làm gì | Đảo lại được không |
|---|---|---|
| `tra_cuu_tai_lieu` | Tìm trong tài liệu buổi học (mục 2), trả đoạn kèm mã mục | Chỉ đọc |
| `goi_y_bai_tap` | Gửi gợi ý làm bài cho một học viên | Gợi ý quá tay thì học viên đã chép |
| `chuyen_TA` | Chuyển câu hỏi cho TA, báo giờ TA trả lời | Được |
| `dang_FAQ` | Đăng câu hỏi và trả lời vào FAQ cả lớp | Gỡ được, nhưng nhiều người đã đọc |

2. **Dữ liệu dựng: tài liệu buổi học và trang khoá học.** Đây là thứ duy nhất trợ lý được dựa vào. Dùng nó để suy ra AI có thể bịa ở đâu, nói sai hạn ở đâu.

Khoá "Nhập môn phân tích dữ liệu", học viên đang học buổi 3.

*Dữ liệu A — Tài liệu buổi 3 "Thống kê mô tả"*

- **§3.1 Trung bình cộng.** Tổng các giá trị chia cho số giá trị. Trung bình bị kéo mạnh bởi giá trị ngoại lai.
- **§3.2 Trung vị.** Giá trị đứng giữa khi sắp xếp dữ liệu tăng dần. Nếu số giá trị là chẵn, trung vị là trung bình cộng của hai giá trị đứng giữa. Trung vị ít bị ảnh hưởng bởi giá trị ngoại lai, nên thường dùng cho dữ liệu lệch như thu nhập hay giá nhà.
- **§3.3 Phương sai và độ lệch chuẩn.** Phương sai đo mức độ phân tán của dữ liệu quanh trung bình. Độ lệch chuẩn là căn bậc hai của phương sai, có cùng đơn vị với dữ liệu. Vì là căn bậc hai, độ lệch chuẩn luôn nhỏ hơn phương sai.
- **§3.4 Giá trị ngoại lai.** Giá trị khác hẳn phần lớn dữ liệu. Trước khi bỏ một giá trị ngoại lai, cần hỏi: đây là lỗi nhập liệu hay là dữ liệu thật?
- **§3.5 Bài tập 3.** Lương tháng của 8 nhân viên một phòng (triệu đồng, dựng): 9, 10, 10, 11, 12, 12, 13, 45. (a) Tính trung bình và trung vị. (b) Con số nào mô tả "lương điển hình" của phòng tốt hơn? Vì sao?

Tài liệu buổi 3 **không có** slide đánh số; chỉ có các mục §3.1 … §3.5.

*Dữ liệu B — Trang khoá học*

| Trường | Giá trị |
|---|---|
| Buổi đã mở | 1, 2, 3 |
| Đề cương các buổi sau | 4 Trực quan hoá dữ liệu · 5 Xác suất cơ bản · 6 Phân phối chuẩn · **7 Kiểm định giả thuyết và p-value** · 8 Hồi quy tuyến tính · 9 Làm sạch dữ liệu · 10 Dự án cuối khoá |
| Công cụ | Bài tập làm bằng bảng tính. |
| Bài tập 2 | Hạn 06/11/2026 23:59 · **đã hết hạn** · đáp án công bố 07/11: "Trung bình cộng của 4, 8, 6, 5, 7 là **6**." |
| Bài tập 3 | Hạn **13/11/2026 23:59** · **đang mở** |
| Giờ TA | 08:00–22:00 |

3. **Ba tin mồi** để bắt đầu viết kịch bản ở chặng 2 (nhóm tự viết thêm, tự đổi):

- *"Trung vị khác trung bình cộng chỗ nào ạ? Khi nào dùng cái nào?"*
- *"Cái độ lệch ấy là gì ạ, em đọc không hiểu."*
- *"Hạn nộp bài 3 là khi nào ạ? Với cả tính độ lệch chuẩn bằng Excel thì dùng hàm gì?"*

**Deliverable.** Bài nộp theo `../03-spec-template.md` + prototype 1–2 màn hình cho lát cắt này (xem `../02-guide.md` §5).

**Demo bắt buộc.** Bấm qua một ca **bài đang mở hoặc tài liệu sai**: trợ lý không chắc thì học viên thấy gì, và TA biết khi nào. Rồi trả lời một **ca lạ** do TA, nhóm bạn hoặc giảng viên đưa tại chỗ.

**Hard tests** *(chỗ dễ sai, nhóm nên có kịch bản cho các loại này)*: Xin giải hộ bài đang mở · xin xác nhận đáp số bài đang mở · tài liệu của khoá có câu sai · học viên nhắc tới nguồn không có ("slide 12") · học viên tự chê mình · hỏi nội dung buổi chưa học.

**Rubric riêng (tham khảo)** — "một sản phẩm tốt nên làm gì" trong domain này. Dùng để tự soát, không phải đáp án. Chấm chung theo `../04-rubric.md`.

1. Chỉ dùng tài liệu buổi đã học; câu về nội dung phải chỉ ra mục (§3.2…).
2. Nội dung buổi sau: nói buổi nào sẽ học, giới thiệu tối đa một câu.
3. Bài đang mở: chỉ gợi ý bước đầu; không đưa, không xác nhận đáp số, không viết nhận xét hộ.
4. Bài đã hết hạn, đã có đáp án: được giải thích đầy đủ.
5. Hạn nộp: chỉ lấy từ trang khoá học.
6. Tài liệu có thể sai: nói câu nào cần xem lại và vì sao, bảo người học báo giảng viên; không khẳng định tài liệu đúng khi chưa kiểm.
7. Không nói về nguồn không có (ví dụ "slide 12"); không nói đã làm việc nó không làm được ("đã báo TA").
8. Không nhận xét năng lực người học.

**Bonus.** Prototype có một lời gọi AI thật trả lời kèm mã mục tài liệu · có màn hình phía TA (hàng chờ câu được chuyển).

**An toàn & đạo đức.** Không đưa, không xác nhận đáp số bài đang mở · không nhận xét năng lực người học · không nói đã làm việc nó không làm được ("đã báo TA") · không dán bài tập thật của khoá AI20K hay trường khác · dữ liệu trong bài là dựng.

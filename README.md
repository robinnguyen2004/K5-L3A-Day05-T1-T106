# LAB5 — Thiết kế sản phẩm AI cho lúc nó sai · AI20K K5 · Ngày 5

**Kịch bản → Khi sai → Mức tự chủ → Prototype → Demo.** Đây không phải bài thi code — đây là bài tập **tư duy sản phẩm AI**: khi AI không chắc hoặc sai thì người dùng thấy gì, và AI được tự làm tới đâu.

## Thành viên nhóm

Bài nộp cần **danh sách thành viên** và **mỗi người làm gì**. Nhóm tự chia việc, không bắt buộc theo vai trò cố định. Ví dụ một cách chia (không bắt buộc): dựng prototype, viết câu chữ AI, giữ bảng kịch bản và kịch bản demo.

**Lớp:** ____ · **Bàn / zone:** ____ · **Track:** ____

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |

> Copy bảng này lên đầu `spec.md` và điền. Ghi cụ thể việc mỗi người làm — khi demo, ai cũng có thể bị hỏi về phần có tên mình.

- Thời lượng: **240 phút**, trong một buổi lab. L3A: 12/11/2026 · L3B: 13/11/2026.
- Nhóm: **đúng 3 người**, cùng một track, nộp **một** bài.
- Lát cắt **phát sẵn** trong từng track — nhóm không phải tìm bài toán, không phỏng vấn, không khảo sát. Toàn bộ thời gian dành cho thiết kế.

## Chuẩn bị trước buổi lab

- Người dựng prototype đã đăng ký sẵn một công cụ: Lovable, v0.dev, Figma, một coding agent như Claude, Codex hoặc Gemini, hoặc chỉ cần trình duyệt nếu làm HTML.
- Không cần chatbot hay API key: đây là bài **thiết kế**, không chạy agent.

## Bắt đầu từ đâu?

1. Đọc **`01-challenge-brief.md`** để hiểu khung chung, rồi chọn **một** track trong **`tracks/`**.
2. Mở **`02-guide.md`** — hướng dẫn từng chặng, đang ở chặng nào đọc mục đó.
3. Viết bài nộp theo **`03-spec-template.md`** — dàn ý ngắn, mỗi mục cần bằng chứng gì và lập luận gì.
4. Đọc **`04-rubric.md`** ngay từ đầu — biết trước bài được chấm theo tiêu chí nào.

| File / thư mục | Nội dung |
|---|---|
| `01-challenge-brief.md` | Đề bài: bảng 5 track · lát cắt · ràng buộc chung · tiêu chí nghiệm thu |
| `02-guide.md` | Hướng dẫn 6 chặng + demo, mỗi chặng mở bằng các câu hỏi nhóm phải tự trả lời |
| `03-spec-template.md` | Dàn ý bài nộp: mỗi mục cần bằng chứng gì, lập luận gì |
| `04-rubric.md` | Rubric 100 điểm + bonus, và checklist TA xác minh ba mốc |
| `tracks/` | **5 track**, cùng một khung mục: T1 Y tế · T2 Giáo dục · T3 Du lịch · T4 Thương mại điện tử · T5 Ví điện tử. Mỗi track có sản phẩm thật để tham khảo, lát cắt, những việc AI có thể làm (tool), tin mồi |
| `prototype-sample/` | Prototype mẫu của giảng viên (track Du lịch): một ca ở ba mức tự chủ. Mở `index.html` bằng trình duyệt. Xem để biết "đủ" trông thế nào — đừng chép giao diện |

## Lịch — 240 phút

| Chặng | Phút | Câu hỏi chính | Mốc TA |
|---|---|---|---|
| 1 · Đọc đề | 10 | Lát cắt này là gì, sai thì ai chịu? | |
| 2 · Lường trước kịch bản | 35 | Ở đâu AI sẽ sai, và sai chỗ nào đắt nhất? | **M1** |
| 3 · **UX khi AI sai** | 45 | Lúc AI không chắc hoặc sai, người dùng thấy gì, làm gì tiếp? | |
| 4 · **Mức tự chủ** | 35 | Việc nào AI tự làm, việc nào hỏi người, việc nào không làm? | **M2** |
| Nghỉ | 15 | | |
| 5 · **Prototype** | 60 | Người dùng có *thấy và bấm* được thiết kế đó không? | **M3** |
| 6 · Tốt là gì | 10 | Ba tiêu chí nào thì dám phát hành? | |
| Demo | 30 | Một khối: demo trong zone, hoặc xung phong trước lớp (bonus) | |

Chặng 3, 4, 5 là phần chính — **140 phút** và phần lớn số điểm. **Thiếu giờ thì bỏ chặng 6 và giảm số kịch bản ở chặng 2; không bao giờ cắt chặng 3, 4, 5.**

Ba mốc M1, M2, M3 để giữ nhịp và cứu nhóm kẹt: TA đi qua bàn, nhìn đúng các ô trong `04-rubric.md` Phần 3, khoảng 1 phút mỗi nhóm.

## Giải thích từng chặng

**Chặng 1–2 · Để làm gì:** biết trước AI sẽ sai ở đâu, trước khi vẽ bất cứ màn hình nào. Nhóm viết kịch bản người dùng nhắn, tự suy ra AI có thể sai thế nào với bộ tool và dữ liệu của track, một sản phẩm tốt nên làm gì, rồi khoanh hai chỗ sai đắt nhất.

**Chặng 3–4 · Để làm gì:** thiết kế hai thứ khó nhất của sản phẩm AI: lúc nó sai thì người dùng thấy gì, và nó được tự làm việc gì. Mỗi lựa chọn phải có lý do theo **cái giá khi sai**.

**Chặng 5 · Để làm gì:** thiết kế trên giấy dễ nói hay; prototype cho thấy người dùng có thực sự thấy và bấm được không. Chỉ **1–2 màn hình**, cho ca khó nhất.

**Chặng 6 · Để làm gì:** chốt "thế nào là đạt" bằng ba tiêu chí chấm được. Làm nhẹ — bộ eval đầy đủ học ở buổi Evaluation.

**Demo · Để làm gì:** cho người khác bấm thử thiết kế của nhóm trên một ca nhóm chưa chuẩn bị.

- Demo là **một khối 30 phút cuối**, không chia vòng cố định. Nhóm demo **trong zone** (TA và các nhóm cùng bàn hoặc cùng zone xem), hoặc **xung phong demo trước lớp** — **nhóm xung phong được cộng điểm bonus** (`04-rubric.md`).
- Người xem (TA, nhóm bạn hoặc giảng viên) đưa **ca lạ** tại chỗ.
- **Mỗi thành viên trình bày một phần.**

## Nộp bài

### Tạo repo mới, không fork repo đề bài

Nhóm tạo một repo GitHub **hoàn toàn mới và trống**. Không fork, không clone repo này rồi push lên: repo nộp bài chỉ chứa bài của nhóm, để người chấm không phải lọc đề ra khỏi bài.

Nhóm chỉ cần lấy **đúng một file** từ repo này: `03-spec-template.md`, copy vào repo mình và đặt tên `spec.md`. Mọi thứ còn lại là tài liệu đọc, mở tại đây là đủ.

### Cách đặt tên repo

```
K5-<lớp>-Day05-<track>-<tên nhóm>
```

| Ví dụ | Của nhóm nào |
|---|---|
| `K5-L3A-Day05-T3-BayNhanh` | Lớp L3A · track T3 Du lịch · nhóm BayNhanh |
| `K5-L3B-Day05-T5-ViAnToan` | Lớp L3B · track T5 Ví điện tử · nhóm ViAnToan |

**Bốn phần đầu bắt buộc đúng.** Lớp là `L3A` hoặc `L3B`; track là `T1` đến `T5`, đúng track nhóm đã nhận.

**Tên nhóm ở cuối đặt gì cũng được**: viết liền, không dấu, không khoảng trắng.

**Repo phải để công khai.** Thử mở bằng cửa sổ ẩn danh: mở được thì mới đúng. Để riêng tư là người chấm không chấm được bài.

### Cấu trúc repo

```
K5-L3A-Day05-T3-BayNhanh/
├── spec.md          ← bài nộp theo 03-spec-template.md, bảng thành viên ở đầu
└── prototype/       ← file HTML hoặc file README.md chứa link prototype, kèm 1–2 ảnh có đánh số các chỗ
```

Bảng thành viên ở đầu `spec.md` copy từ đầu file README này. **Mã học viên phải đúng**: đây là căn cứ đối chiếu điểm.

Nộp **link repo** trước **23:59 ngày 12/11/2026** (lớp L3A) · trước **23:59 ngày 13/11/2026** (lớp L3B). Kênh nộp: giảng viên thông báo trong buổi.

## Chấm điểm

Tổng **100 điểm + tối đa 8 điểm bonus**. Chi tiết từng ý điểm: `04-rubric.md`.

| Khối | Điểm | Chấm trên |
|---|---|---|
| R1 · Lát cắt & phạm vi | 5 | `spec.md` §1 |
| R2 · Kịch bản rủi ro | 10 | `spec.md` §2 |
| R3 · Khi AI sai | 25 | `spec.md` §3 |
| R4 · Mức tự chủ | 25 | `spec.md` §4 |
| R5 · Prototype | 25 | `spec.md` §5 + `prototype/` + demo |
| R6 · Tốt là gì | 5 | `spec.md` §6 |
| R7 · Demo & phân công | 5 | demo |
| **Bonus** | **+8** | xung phong demo trước lớp · prototype có lời gọi AI thật |

**75 trên 100 điểm nằm ở R3, R4, R5** — khi AI sai, mức tự chủ, và prototype thể hiện hai thứ đó. Điểm chấm **lập luận cho từng quyết định** (đã cân nhắc gì, chọn gì, vì sao, loại gì), không chấm điền đủ ô.

## Luật chung

1. **Không chat với sản phẩm thật.** Sản phẩm thật trong track chỉ để tham khảo. Mọi kịch bản do nhóm tự lường trước từ lát cắt, bộ tool và dữ liệu dựng của track.
2. **Vibe-coding rule:** dùng AI để build thoải mái, nhưng không giải thích được phần có tên mình thì phần đó 0 điểm (TA, giảng viên hỏi bất kỳ thành viên khi demo).
3. Chỉ dùng dữ liệu dựng trong track hoặc dữ liệu giả tự sinh — **không dùng dữ liệu thật của người thật**: không số tài khoản, số thẻ, OTP, thông tin sức khoẻ, đơn hàng thật.
4. Không commit API key; key để biến môi trường.
5. Prototype có lời gọi AI thật thì chỉ đưa dữ liệu dựng vào — công cụ free tier có thể dùng dữ liệu để huấn luyện.
6. Tình huống, lời người dùng và chính sách trong bài là **dựng để học**. Khoá học không liên kết với các công ty có sản phẩm được nhắc tới.

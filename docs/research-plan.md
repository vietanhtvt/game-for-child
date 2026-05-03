# Nghiên cứu & Kế hoạch — Game Tiền Tiểu Học cho trẻ 3–6 tuổi (Việt Nam)

_Cập nhật: 2026-05-02 (Iteration 2 — bổ sung dữ liệu mùa tuyển sinh 2026–2027, khung quốc tế, phương pháp đánh giá game)_

Mục tiêu: Xây dựng một game giáo dục giúp trẻ 3–6 tuổi ở Việt Nam (trọng tâm Hà Nội) chuẩn bị vào lớp 1 — bám sát kỳ vọng của các trường tư "hot" và Chương trình GDPT 2018 / Chương trình GDMN, đồng thời giữ tinh thần "học mà chơi".

> **Lịch sử cập nhật:**
> - 2026-04-19 (v1): Bản nghiên cứu khởi tạo — bối cảnh, phân loại trường, mini-game, phương pháp NC.
> - 2026-04-21 (v1.1): Bổ sung Ngôi Sao Hà Nội, mini-game không gian/cảm xúc.
> - 2026-05-02 (v2): Crawl mới mạng xã hội mùa 2026–2027, thêm BVIS/Olympia/Wellspring, phân tích sâu 4 trường top, khung quốc tế (EYFS, Cambridge BASE/ASPECTS), heuristic NN/g, TAM cho đánh giá game, Luật Giáo dục sửa đổi 2025, Thông tư 29/2024.

---

## 1. Tổng quan về "tiền tiểu học" ở Hà Nội (từ mạng xã hội & báo chí)

**Bối cảnh chính sách 2025–2026:**
- Chương trình GDPT 2018 được thiết kế để trẻ vào lớp 1 làm quen từ đầu, nên **không học tiền tiểu học trẻ vẫn có thể theo kịp**. Tuy nhiên phụ huynh vẫn đổ xô đi "luyện" vì tâm lý và vì các trường tư có bài đánh giá đầu vào (giaoduc.net.vn; vov2.vov.vn).
- **Thông tư 29/2024/TT-BGDĐT** về dạy thêm – học thêm hiệu lực **14/02/2025**: chỉ tổ chức khi học sinh có nhu cầu, tự nguyện, có đồng ý của phụ huynh; **không được dạy trước chương trình** (vneconomy.vn, giaoduc.net.vn). Mặc dù trên giấy tờ Thông tư 29 hướng đến cấp tiểu học trở lên, tinh thần này thường được áp dụng cho cả lớp tiền tiểu học.
- **Luật Giáo dục sửa đổi 2025 (số 123/2025/QH15)** có hiệu lực từ **01/01/2026**, kèm 8 thay đổi lớn (luatnguyen.vn) — củng cố quan điểm không dạy trước chương trình, đề cao quyền học tập đúng độ tuổi của trẻ.
- Lịch tuyển sinh công lập Hà Nội 2025–2026: mầm non/lớp 1/lớp 6 từ 04–06/07/2025 (tapchigiaoduc.edu.vn). Hệ thống đăng ký trực tuyến đầu cấp: `tsdaucap.hanoi.gov.vn` (hanoimoi.vn).
- Mức học phí thị trường lớp tiền tiểu học (sentiaschool.edu.vn, giasudantri.vn, lienviet.edu.vn):
  - Tại trường mầm non thuê GV ngoài: **~500.000đ/tháng**.
  - Gia sư tại nhà: 200–400k/buổi, 2–3 buổi/tuần.
  - Trung tâm chuyên (KioMath, UCMAS, Sentia, EduLive): 1.5–3 triệu/khóa 8–12 buổi.

**Các nhóm kỹ năng phụ huynh & chuyên gia nhắc đến nhiều nhất (đối chiếu nhiều nguồn):**
1. **Kỹ năng xã hội – cảm xúc**: độc lập, tự phục vụ, làm chủ hành vi, hợp tác, chia sẻ. Marie Curie nhấn mạnh: nếp ăn, nếp ngủ, tự lực, đồng đội (mariecuriehanoischool.com).
2. **Kỹ năng học tập nền tảng**: tập trung chú ý lâu, làm theo chỉ dẫn nhiều bước, ghi nhớ ngắn hạn.
3. **Tiền đọc – tiền viết**: nhận mặt 29 chữ cái, dấu thanh, ghép vần cơ bản, cầm bút, tô/viết nét.
4. **Tiền toán**: đếm 1–10 (hoặc 1–20), so sánh nhiều/ít, nhận diện hình, quy luật, không gian.
5. **Tiếng Anh làm quen** (với các trường song ngữ/Cambridge): nghe hiểu chỉ dẫn, từ vựng cơ bản, mạnh dạn nói.
6. **Vận động tinh**: cầm bút, nối hình, tô màu — ảnh hưởng trực tiếp đến viết chữ.
7. **Tư duy logic**: Vinschool 2026–2027 nâng tách riêng "Tư duy logic" thành một bài đánh giá độc lập (vinschool.edu.vn).

**Insight từ social listening (Facebook, TikTok, diễn đàn — crawl 2026-04 đến 2026-05):**
- **Nỗi đau lặp lại** của phụ huynh:
  - "Con không chịu ngồi yên 30 phút" → vào lớp 1 sợ bị cô phê bình.
  - "Con cầm bút sai cách, viết nét yếu" — nỗi lo lớn nhất sau IQ.
  - "Con sợ thi", "Con khóc khi bị hỏi GV nước ngoài" — tâm lý phòng thi.
  - "Bố mẹ hỏi nhau lò luyện nào ‘chuẩn’ Archimedes/ĐTĐ" — thị trường lò luyện rất sôi động dù bị chính các trường phản đối.
- **Cộng đồng lớn nhất**:
  - **Con Tự Học (contuhoc.com)** ~175k thành viên, 3 group Facebook chính thức (contuhoc.com).
  - **Lamchame.com** — diễn đàn riêng từng trường (Vinschool Times City, Archimedes, Nguyễn Siêu).
  - **Webtretho** — chia sẻ kinh nghiệm thi, bài thi mẫu.
  - **Hội cha mẹ chuẩn bị cho con vào lớp 1 Hà Nội** (Facebook group, được dẫn lại nhiều ở dantri, znews).
- **Hashtag TikTok / YouTube Shorts** mà phụ huynh tìm: `#tientieuhoc`, `#vaolop1`, `#dethilop1`, `#luyenthilop1`, `#chuanbivaolop1`, `#dethiarchimedes`, `#nguyensieu`, `#doanthidiem`, `#vinschool`.
- **Bài viết được chia sẻ nhiều nhất** (tham khảo cho hiểu nỗi đau): afamily.vn ("Cho con vào lớp 1: bà mẹ Hà Nội chia sẻ tất tần tật"), cafef.vn ("Cô giáo tiếng Anh có con đỗ 4 trường tiểu học chia sẻ kinh nghiệm"), VnExpress ("Phụ huynh cho con học trước lớp 1 cả năm").

---

## 2. Phân loại các trường tiểu học "hot" ở Hà Nội theo hình thức tuyển sinh

### 2.1 Bảng phân loại tổng hợp

| Nhóm | Trường tiêu biểu | Hình thức đánh giá đầu vào | Mức độ "khó" | Học phí (triệu/năm) |
|---|---|---|---|---|
| **A. Trường có bài ĐGNL/IQ + phỏng vấn đa kỹ năng** | Nguyễn Siêu (CS/CI), Archimedes Academy (AAPS, ADAS), Ngôi Sao Hà Nội, Đoàn Thị Điểm (CLC & Cambridge), Vinschool (hệ Nâng cao/Cambridge) | IQ/tư duy, Tiếng Việt, Toán, Tiếng Anh (có GV nước ngoài), phỏng vấn, trò chơi quan sát | Cao | 60–250 |
| **B. Trường đánh giá qua CLB trải nghiệm nhiều buổi** | Nguyễn Siêu ("Spark in Me" — nhiều buổi CLB tiền tiểu học), Archimedes ("LET'S START" — 5 buổi ARCERS' FIRST STEPS, 8 buổi 2024–2025), Ngôi Sao ("LET'S START 2026") | Quan sát 6 nhóm kỹ năng qua hoạt động nhóm, không phải 1 bài thi duy nhất | Trung bình – cao | 60–250 |
| **C. Trường không kiểm tra kiến thức, chỉ trải nghiệm/phỏng vấn nhẹ** | Marie Curie (tuyên bố không kiểm tra Tiếng Việt/Toán/Tiếng Anh, khuyên phụ huynh không đi "lò luyện") | Trải nghiệm "một ngày là HS lớp 1" → quan sát tập trung, tự tin, hợp tác, tự lực, nếp ăn ngủ, đồng đội | Thấp | 50–80 |
| **D. Trường công chất lượng cao theo tuyến** | Lê Quý Đôn (Nam Từ Liêm), trường CLC quận, các trường tuyến công | Theo tuyến + phỏng vấn nhẹ | Thấp – trung bình | Miễn phí – 5 |
| **E. Trường quốc tế / song ngữ phương Tây** | BVIS (Nord Anglia), Wellspring, The Olympia Schools, GIS, ISHCMC-AA, Concordia | Phỏng vấn tiếng Anh, quan sát chơi, có thể yêu cầu báo cáo trường mầm non; ít coi trọng kiến thức Toán/Tiếng Việt | Trung bình (focus EQ + tiếng Anh) | 200–870 |

### 2.2 Cập nhật chi tiết mùa tuyển sinh 2026–2027

| Trường | Mốc thời gian quan trọng | Số lớp/sĩ số | Nội dung kiểm tra mới (so với 2025–2026) | Nguồn |
|---|---|---|---|---|
| **Nguyễn Siêu (NSPS)** | "Một ngày là HS lớp 1 – Spark in Me": **CS 01/10/2026**, **CI 11/01/2026**; thông báo PDF tuyển sinh 2026–2027 | 10 lớp, ≤ 30 HS/lớp | Tiếng Anh (Cambridge), Tiếng Việt, kỹ năng cơ bản — đánh giá qua hoạt động CLB, không phải bài thi viết | nsps.edu.vn (PDF tuyển sinh 2026-2027) |
| **Archimedes (aschool.edu.vn)** | **A/TEST 2025–2026** (khảo sát NL HS ngoài hệ thống); **LET'S START 2026** — 5 buổi ARCERS' FIRST STEPS từ T3–15/05/2026; "Backpack to Grade 1" mở từ 07/12/2025 | 360 HS THCS + 340 HS TH/THCS/THPT (Đông Anh) | 3 môn (Toán, TV, Anh) + bài nâng cao Toán/Anh, **trắc nghiệm + tự luận**; lịch khảo sát 09/03 và 13/03/2025 | aschool.edu.vn, dantri, congdankhuyenhoc |
| **Vinschool** | Tuyển sinh 2026–2027 đăng trên `vinschool.edu.vn/news_event/tuyen-sinh-tieu-hoc/` | Theo cơ sở (Times City, Smart City, Ngôi Sao, Ocean Park…) | **Hệ Chuẩn**: Tư duy logic + Phỏng vấn TV. **Hệ Nâng cao**: Tư duy logic + Phỏng vấn TV và TA. Tách riêng "Tư duy logic" là điểm mới so với 2024 | vinschool.edu.vn, webtuyensinh.com |
| **Đoàn Thị Điểm** | Bộ đề luyện CLC & Cambridge **2026** đã được đăng công khai (tailieuonthi.io.vn) | Theo cơ sở (Mỹ Đình, Ecopark) | Cấu trúc 4 phần ổn định: IQ → Toán → Tiếng Việt → Kể chuyện; giữ nguyên dạng "khoanh hình khác biệt", "tìm mảnh ghép thiếu", "chọn ô theo quy luật" | tailieuonthi.io.vn, znews, victoryschoolbmt.edu.vn |
| **Marie Curie** | Trải nghiệm "1 ngày là HS lớp 1" — không kiểm tra kiến thức | Đa cơ sở (Mỹ Đình, Việt Hưng, Long Biên, Hoàng Đạo Thúy) | Quan sát: tập trung, tự tin, hợp tác, tự lực, nếp ăn ngủ, năng khiếu, đồng đội | mariecuriehanoischool.com |
| **Ngôi Sao Hà Nội (situ.edu.vn)** | LET'S START 2026, tuyển sinh lớp 1 năm 2026–2027: **9 lớp, 32 HS/lớp**, lớp 1B0 thêm KT tiếng Anh | 9 lớp × 32 HS | 3 môn Toán/TV/TA; có học bổng Ngôi Sao | ngoisaohanoi.edu.vn, situ.edu.vn, mathx.vn |
| **Wellspring Hà Nội** | Tuyển sinh **2025–2026 + 2026–2027** từ lớp 1–12 (`tuyensinh.wellspring.edu.vn`) | Đa cấp | Hệ song ngữ – phỏng vấn tiếng Anh & TV, quan sát qua workshop trải nghiệm | tuyensinh.wellspring.edu.vn |
| **The Olympia Schools** | Tuyển sinh 2025–2026 (`event.theolympiaschools.edu.vn`) | Liên cấp | Phỏng vấn + workshop trải nghiệm, học phí 138–868 triệu/năm | event.theolympiaschools.edu.vn |
| **BVIS (British Vietnamese International School)** | Tuyển sinh quanh năm theo Nord Anglia | Theo lớp/cơ sở | Phỏng vấn tiếng Anh là chính, tham chiếu EYFS | gpaenglish.edu.vn, dantri |

Nguồn tổng hợp: nhandan.vn, dantri.com.vn, congdankhuyenhoc.vn, thanhnien.vn, tienphong.vn, znews.vn, afamily.vn, nsps.edu.vn, aschool.edu.vn, situ.edu.vn, ngoisaohanoi.edu.vn, vinschool.edu.vn, mariecuriehanoischool.com, tuyensinh.wellspring.edu.vn, theolympiaschools.edu.vn, haysiri.com, lamchame.com.

---

## 3. Phân tích nội dung bài kiểm tra (dựa trên đề thi thử & chia sẻ thực tế)

### 3.1 Tiếng Việt / Ngôn ngữ
- Nhận mặt chữ cái in hoa – in thường, dấu thanh.
- Ghép vần đơn giản (ba, bà, má…), phân biệt nguyên âm/phụ âm.
- Đọc hiểu đoạn ngắn, mở rộng vốn từ.
- **Kể chuyện theo tranh**: cô giáo kể 1 câu chuyện → trẻ trả lời trắc nghiệm A/B/C/D (Nguyễn Siêu).
- **Sắp xếp các bức tranh kể lại thành câu chuyện** (Đoàn Thị Điểm — 4–6 tranh).

### 3.2 Toán tư duy
- Đếm số, thêm/bớt trong phạm vi 10–20.
- So sánh số lượng, nối ô theo số lượng.
- **Tìm quy luật hình/dãy số** ("hình kế tiếp là hình nào?").
- Hình học cơ bản: nhận dạng, gộp/tách, đối xứng.
- **Nối các dấu chấm để tạo hình** (ngôi nhà, con vật).
- Định hướng không gian (trên/dưới, trái/phải, trong/ngoài).

### 3.3 IQ / Quan sát / Trí nhớ — chi tiết hóa từ đề ĐTĐ & Archimedes 2026
Đoàn Thị Điểm chuẩn hoá 3 dạng IQ cho lớp 1 (znews, victoryschoolbmt.edu.vn):
1. **Khoanh tròn vào hình không giống** với các hình còn lại (find the odd one out).
2. **Hình chữ nhật bị thiếu 1 miếng**, trẻ tìm miếng ghép kín phần bị thiếu (matrix completion / "?" missing piece).
3. **Quan sát các hình vẽ được sắp xếp theo quy luật**, chọn 1 ô thích hợp để nối vào vị trí dấu chấm (Raven-style 2x2 / 3x3 matrix).

Ngoài ra:
- Tìm hai hình giống nhau, **tìm điểm khác nhau giữa hai tranh** (3–5 điểm).
- Trò chơi **tìm vật bị giấu** (Nguyễn Siêu) — kiểm tra working memory.
- **Nối theo mẫu**, chọn mảnh ghép phù hợp với dấu "?".
- Test IQ "10 câu trắc nghiệm" công khai trên `download.vn`.

### 3.4 Tiếng Anh (trường song ngữ/Cambridge)
- GV nước ngoài hỏi từ vựng theo bộ đề, hỏi tới khi trẻ không trả lời được.
- Nghe kể chuyện ngắn → trả lời câu hỏi liên quan.
- Làm theo chỉ dẫn ("touch the…", "show me…"), thi nhóm 4 bạn.
- Vinschool Cambridge: phỏng vấn 1-1 với GV nước ngoài; Nguyễn Siêu CI: theo chuẩn Cambridge Early Years.

### 3.5 Kỹ năng mềm được quan sát ngầm
- Tự tin, giao tiếp bằng mắt, diễn đạt mạch lạc.
- Làm theo chỉ dẫn nhiều bước.
- Kiên trì khi gặp câu khó, không bỏ cuộc.
- Chờ đến lượt, hợp tác trong nhóm.
- **Marie Curie**: nếp ăn, nếp ngủ, tính đồng đội, năng khiếu (gợi ý mở rộng game qua module "Một ngày của bé").

### 3.6 Ma trận nội dung × trường (skill-coverage map)

| Kỹ năng / Dạng câu hỏi | Archimedes | Nguyễn Siêu | ĐTĐ (CLC) | ĐTĐ Cambridge | Vinschool Std | Vinschool NC | Ngôi Sao | Marie Curie |
|---|---|---|---|---|---|---|---|---|
| Đếm 1–20 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| So sánh số lượng | ✅ | — | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| Quy luật hình | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| Hình khác biệt (odd one out) | ✅ | — | ✅ | ✅ | — | — | — | — |
| Mảnh ghép thiếu | — | ✅ | ✅ | ✅ | — | — | — | — |
| Nhận chữ cái VN | ✅ | ✅ | ✅ | — | ✅ | ✅ | ✅ | — |
| Ghép vần | ✅ | — | ✅ | — | ✅ | ✅ | ✅ | — |
| Sắp xếp tranh kể chuyện | — | ✅ | ✅ | ✅ | — | — | — | — |
| Nghe → chỉ tranh | — | ✅ | — | — | — | — | ✅ | — |
| Phỏng vấn TA 1-1 | — | ✅ | — | ✅ | — | ✅ | ✅ (lớp 1B0) | — |
| Trí nhớ làm việc (vật bị giấu) | — | ✅ | — | — | ✅ (qua tranh) | ✅ | — | — |
| Tư duy logic độc lập | — | — | — | — | ✅ | ✅ | — | — |
| Quan sát kỹ năng xã hội | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Tự lực / nếp sinh hoạt | — | ✅ | — | — | — | — | — | ✅ |

> Heatmap này dùng để xác định **mini-game nào đang thiếu** trong game của chúng ta (xem mục 4.3).

---

## 4. Bản đồ tính năng game (đối chiếu với nội dung kiểm tra)

### 4.1 Danh sách mini-game

Mỗi tính năng = 1 "mini-game". Tất cả đều thiết kế dưới 3–5 phút/lượt để phù hợp khả năng tập trung của trẻ.

| Chủ đề | Mini-game đề xuất | Kỹ năng chính | Trường tham chiếu |
|---|---|---|---|
| Chữ cái | "Bắt chữ": ghép chữ cái với hình (A–Áo, B–Bò) | Nhận mặt chữ, liên hệ âm-hình | Archimedes, ĐTĐ |
| Ghép vần | "Nhà ghép vần": kéo phụ âm + nguyên âm + thanh | Tiền đọc | Archimedes |
| Đếm & số | "Chợ quê": đếm – gắn số | Tiền toán | ĐTĐ, Vinschool |
| So sánh | "Cân voi": nặng hơn / nhẹ hơn, nhiều hơn / ít hơn | Tiền toán | Chung |
| Quy luật | "Đoàn tàu họa tiết": chọn toa tiếp theo | Logic, IQ | Nguyễn Siêu, Archimedes, Vinschool NC |
| Hình học & không gian | "Xếp Tangram mini" | Quan sát, không gian | Chung |
| Tìm khác biệt | "Hai bức tranh": tìm 3–5 điểm khác | Quan sát | ĐTĐ |
| Trí nhớ | "Lật thẻ đôi" theo chủ đề con vật / trái cây | Trí nhớ làm việc | Nguyễn Siêu, Vinschool |
| Kể chuyện | Xem hoạt hình ngắn → sắp xếp 4 tranh theo trình tự → trả lời trắc nghiệm | Nghe hiểu, logic kể chuyện | Nguyễn Siêu, ĐTĐ |
| Vận động tinh | "Tô theo nét", "nối các dấu chấm" | Chuẩn bị viết | ĐTĐ |
| Tiếng Anh | "Touch & Say": chạm đúng từ/vật, nghe chỉ dẫn đơn giản | Nghe hiểu, từ vựng cơ bản | Nguyễn Siêu, Vinschool Cambridge |
| Cảm xúc – xã hội | "Bé cảm xúc hôm nay": nhận diện – đặt tên cảm xúc qua gương mặt | EQ | Toàn bộ |

### 4.2 Mini-game đề xuất bổ sung (sau iteration v2)

| Chủ đề | Mini-game mới | Lý do bổ sung | Trường tham chiếu |
|---|---|---|---|
| **Hình khác biệt** | "Đại tiệc hình thù": gạch bỏ hình lạc ra khỏi hàng (4–6 hình/hàng) | Lấp ô trống "odd one out" cho ĐTĐ — đang thiếu | ĐTĐ CLC & Cambridge |
| **Mảnh ghép thiếu (matrix)** | "Mảnh ghép kỳ diệu": chọn miếng đúng cho ô "?" trong ma trận 2x2 / 3x3 | Lấp ô "Raven" cho ĐTĐ & Nguyễn Siêu — chưa có | ĐTĐ, Nguyễn Siêu |
| **Tìm vật bị giấu** | "Trốn tìm trong nhà": ghi nhớ vị trí 3–5 đồ vật → tìm sau khi bị che | Working memory được Nguyễn Siêu kiểm tra — khác với "lật thẻ đôi" | Nguyễn Siêu |
| **Nếp sinh hoạt** | "Một ngày của bé": kéo thả các hoạt động (đánh răng, ăn sáng, đi học, ngủ) theo đúng trình tự | Marie Curie & Nguyễn Siêu coi trọng tự lực | Marie Curie, Nguyễn Siêu |
| **Tư duy logic độc lập** | "Bộ ba cùng nhóm": phân loại 6 vật vào đúng nhóm (động vật / phương tiện / thực phẩm) | Vinschool 2026–2027 tách bài "Tư duy logic" | Vinschool NC |
| **Phát âm tiếng Anh** | "Nhắc lại theo cô": ghi âm trẻ phát âm 5 từ → so sánh với mẫu (nếu hợp lệ về quyền dữ liệu) | Vinschool Cambridge & NSPS CI phỏng vấn 1-1 — cần luyện trước | Vinschool NC, NSPS CI |
| **Hợp tác (mô phỏng)** | "Lượt của ai?": trò chơi turn-taking với 2 nhân vật ảo, bé phải chờ đến lượt | Quan sát ngầm "chờ đến lượt" của tất cả các trường | Toàn bộ |

### 4.3 Nguyên tắc thiết kế chung
- Giọng đọc tiếng Việt chuẩn Bắc Bộ + giọng tiếng Anh bản ngữ (kèm tuỳ chọn Anh-Anh / Anh-Mỹ).
- Luôn có hướng dẫn bằng âm thanh (trẻ 3–4 tuổi chưa đọc được).
- Phản hồi tích cực: không dùng chữ "SAI", thay bằng "Thử lại nào!".
- Level adaptive theo tuổi (3, 4, 5, 6) — cùng 1 mini-game có 3–4 độ khó.
- Session ngắn, có nhắc nghỉ mắt sau 15 phút (theo khuyến cáo WHO ≤ 1h/ngày với trẻ 2–5).
- Không quảng cáo, không mua trong ứng dụng hướng đến trẻ.
- **Heuristic NN/g cho UX trẻ em** (nngroup.com — UX Design for Children Ages 3–12, 4th Ed.):
  - Nút bấm tối thiểu **44×44px**, nên ≥ 60×60px cho nhóm 3–5 tuổi do motor chưa hoàn thiện.
  - Tránh thao tác "double tap"; ưu tiên drag-drop và tap đơn.
  - Phản hồi ngay (< 1s), có hiệu ứng âm thanh + animation.
  - Không dùng metaphor lạ (ví dụ icon "save = đĩa mềm").
  - **Game hoá có chừng mực**: badge và phần thưởng nhỏ giúp tăng động lực, nhưng tránh hệ thống điểm thưởng tạo áp lực thi đua.

---

## 5. Phương pháp nghiên cứu bổ sung để cải thiện tính năng

Để game thực sự hữu ích và khác biệt, đề xuất triển khai song song 8 hướng nghiên cứu (mở rộng từ 7 ở v1).

### 5.1 Nghiên cứu định tính với phụ huynh & giáo viên
- **Phỏng vấn sâu 8–12 phụ huynh** có con vừa thi/đang ôn lớp 1 tại 5 nhóm trường (A/B/C/D/E ở mục 2).
- **Phỏng vấn 3–5 giáo viên mầm non** và giáo viên lớp 1 về "gap" thực tế giữa trẻ học trước và không.
- **Nhật ký phụ huynh (diary study)** 2 tuần: ghi lại hoạt động ôn cùng con, điểm khó chịu, app/sách đang dùng.
- **Khung phỏng vấn**: dùng JTBD (Jobs-to-be-Done) để hỏi "khi nào… tôi cần con… để tôi có thể…" — tách rạch ròi nhu cầu thực sự của phụ huynh khỏi thị trường lò luyện.

### 5.2 Thu thập dữ liệu từ mạng xã hội (social listening)
- Crawl & phân tích các nhóm Facebook lớn: "Hội cha mẹ chuẩn bị cho con vào lớp 1 Hà Nội", "Con tự học" (~175k thành viên), webtretho, lamchame.com.
- Dùng NLP / LLM phân cụm chủ đề (topic modelling, BERTopic) để tìm nỗi đau lặp lại: "con không chịu ngồi yên", "con sợ thi"…
- Theo dõi hashtag TikTok/YouTube Shorts: `#tientieuhoc`, `#vaolop1`, `#dethilop1`, `#luyenthilop1`, `#chuanbivaolop1`, `#dethiarchimedes`, `#nguyensieu`, `#doanthidiem`, `#vinschool`.
- **Sentiment analysis** theo quý: theo dõi mood phụ huynh quanh các mốc tuyển sinh (Q4 = đỉnh lo lắng).

### 5.3 Phân tích đề thi thật
- Tập hợp ≥ 30 đề thi thử/đề thật từ Archimedes, Nguyễn Siêu, Đoàn Thị Điểm, Vinschool, Ngôi Sao (VnDoc, tailieuonthi, situ.edu.vn, mathx.vn).
- Gắn nhãn theo ma trận: kỹ năng × độ khó × định dạng câu hỏi (xem 3.6).
- Xuất ra "skill coverage heatmap" để thấy mini-game nào đang thiếu (đã cập nhật ở 3.6 + 4.2).
- **Phương pháp gắn nhãn**: dùng schema có sẵn ở `data/schema/` + LLM-assisted tagging với double-check thủ công.

### 5.4 Playtest có kiểm soát với trẻ
- **Wizard-of-Oz prototype** bằng giấy trước, sau đó prototype số.
- Mỗi phiên 20 phút với 1 trẻ + 1 phụ huynh quan sát, theo độ tuổi 3 / 4 / 5 / 6 (ít nhất 4–6 trẻ mỗi nhóm).
- Đo: thời gian hoàn thành, tỉ lệ đúng, số lần cần gợi ý, biểu cảm (vui/chán), câu hỏi trẻ đặt ra.
- Áp dụng **heuristic thiết kế cho trẻ em** (Nielsen Norman Group, Fisher-Price Play Lab).
- **Think-aloud thân thiện**: với trẻ 3–4 tuổi không nói thành tiếng được → quay video biểu cảm và thao tác, gắn nhãn cảm xúc theo Ekman 6 cảm xúc cơ bản.
- **Co-design**: cho trẻ vẽ hoặc chọn nhân vật yêu thích để đảm bảo cảm giác sở hữu (sense of ownership).

### 5.5 Đối chiếu khung chuẩn giáo dục
- So khớp tính năng với **Chương trình Giáo dục Mầm non 2021** của Bộ GD&ĐT (5 lĩnh vực phát triển: thể chất, nhận thức, ngôn ngữ, tình cảm – kỹ năng xã hội, thẩm mỹ — Thông tư 17/2009 + Văn bản hợp nhất số 01/2021/VBHN-BGDĐT).
- Tham chiếu **Chuẩn phát triển trẻ 5 tuổi** (Bộ GD&ĐT) — 120 chỉ số.
- Tham chiếu khung quốc tế:
  - **EYFS (Early Years Foundation Stage – UK)**: 7 vùng học (Communication & Language, Physical, PSED, Literacy, Maths, Understanding the World, Expressive Arts).
  - **Cambridge Early Years**: chương trình + công cụ đánh giá **ASPECTS** (3–4 tuổi) và **BASE** (4–5 tuổi) — bài đánh giá baseline 15–20 phút, dạng truyện kể, **adapt theo khả năng** của trẻ (cambridge.org/insight) — ý tưởng tốt cho onboarding game của chúng ta.
  - **Montessori Sensitive Periods**: order, language, sensory refinement, small objects, social behavior.
  - **HighScope Key Developmental Indicators (KDIs)**: 58 chỉ số phát triển.
- **Sản phẩm**: mapping table 3 cột — tính năng game ↔ chỉ số CT GDMN 2021 ↔ chỉ số EYFS/Cambridge — minh chứng cho phụ huynh & nhà trường.

### 5.6 Phân tích sản phẩm đối thủ
- Benchmark các app đang được dùng nhiều ở VN: Monkey Junior, VMonkey, Monkey Math, Kyna For Kids, Kids UP (KidsUp), ClassIn Kids, Khan Academy Kids, Lingokids, KioMath, UCMAS.
- Lập bảng so sánh: độ phủ kỹ năng, chất lượng tiếng Việt (giọng vùng miền, từ địa phương), UX cho trẻ 3–4 tuổi (chưa đọc được), mô hình trả phí, screen-time control, tính năng dành cho phụ huynh.
- **Insight quan trọng từ benchmarking**:
  - **KidsUp** dựa trên Montessori + Glenn Doman, có > 100k hoạt động, là app đầu tiên có **screen-time alert sau 10 phút** — chuẩn để chúng ta vượt qua.
  - **VMonkey** bám sát SGK GDPT mới, mạnh về Tiếng Việt — chúng ta nên cộng tác hoặc khác biệt rõ thay vì cạnh tranh trực diện.
  - **Khan Academy Kids** — UX tham chiếu hàng đầu thế giới, free, nhưng 100% Tiếng Anh — cơ hội cho phiên bản VN.
  - Không có app nào hiện tại **bám sát đề thi vào lớp 1 trường tư VN** — đây là góc khác biệt của chúng ta.

### 5.7 Đánh giá game qua mô hình lý thuyết (mới ở v2)
- **TAM (Technology Acceptance Model)** dành cho phụ huynh: đo Perceived Usefulness, Perceived Ease of Use, Attitude, Behavioral Intention bằng survey 20 câu (Likert 1–5). Tham khảo các nghiên cứu Vietnamese về "digital math games" tại VJE 2024 (tcgd.tapchigiaoduc.edu.vn).
- **GameFlow / EGameFlow** cho trẻ: đo concentration, challenge, autonomy, immersion, social interaction → dùng thang đo dành cho trẻ (ảnh emoji thay vì số).
- **Game-Based Assessment (GBA)** framework (classin.vn): xác định mục tiêu → bằng chứng cần thu → phương pháp đo → cải thiện. Áp dụng cho mỗi mini-game.
- **MEEGA+** (Model for Evaluation of Educational Games) — bảng kiểm 31 mục về Usability + Player Experience.

### 5.8 Dữ liệu sử dụng & học tập thích ứng (sau MVP)
- Thu thập ẩn danh: thời lượng chơi, độ chính xác, bỏ cuộc ở bước nào.
- Mô hình **adaptive difficulty** đơn giản (Item Response Theory hoặc Elo) để đề xuất mini-game kế tiếp.
- Dashboard cho phụ huynh: báo cáo tuần theo 5 lĩnh vực phát triển GDMN 2021 + **bảng so sánh ngầm** với chỉ số trung bình theo độ tuổi (không xếp hạng, không gây áp lực).
- **Knowledge tracing** đơn giản (BKT — Bayesian Knowledge Tracing) cho từng kỹ năng → recommend câu hỏi kế tiếp.

---

## 6. Đề xuất các giai đoạn triển khai

| Giai đoạn | Thời lượng | Mốc chính |
|---|---|---|
| **0. Nghiên cứu sâu** | 3–4 tuần | Hoàn tất mục 5.1, 5.2, 5.3, 5.5, 5.6 → spec sản phẩm + skill-coverage heatmap |
| **1. Prototype giấy + test** | 2 tuần | 3 mini-game lõi: Bắt chữ, Đếm, Quy luật + 1 mini-game mới (Mảnh ghép thiếu) |
| **2. MVP số** | 6–8 tuần | 6 mini-game + onboarding kiểu BASE (story-based 15 phút) + báo cáo phụ huynh |
| **3. Playtest mở rộng** | 3 tuần | 15–20 trẻ, iterate UI/UX, đo TAM với phụ huynh |
| **4. Beta công khai** | 4 tuần | Mời ~100 gia đình Hà Nội; thu telemetry; cộng tác với 1–2 trung tâm tiền tiểu học (KioMath / UCMAS / Sentia) |
| **5. Ra mắt v1** | — | Bổ sung Tiếng Anh & kể chuyện, adaptive difficulty (BKT/Elo), phiên bản giọng miền Nam |

---

## 7. Rủi ro & lưu ý đạo đức

- **Không hứa hẹn "đỗ trường top"** — trái tinh thần của chính các trường (Marie Curie công khai phản đối lò luyện) và của Thông tư 29/2024 + Luật Giáo dục sửa đổi 2025. Communications phải dùng từ "chuẩn bị" / "đồng hành", tránh "luyện thi" / "đảm bảo đỗ".
- **Bảo vệ trẻ em**: tuân thủ Luật Trẻ em VN 2016 + Luật Giáo dục sửa đổi 2025, không thu thập dữ liệu định danh của trẻ dưới 7 tuổi; mọi tài khoản gắn với phụ huynh; **không thu thập giọng nói/ảnh** của trẻ trừ khi có consent rõ ràng và mục đích thiết yếu.
- **Thời gian màn hình**: Viện Nhi TƯ & WHO khuyến cáo ≤ 1h/ngày với trẻ 2–5 tuổi — thiết kế session ngắn, có khóa thời gian (theo gương KidsUp với cảnh báo sau 10 phút).
- **Ngôn ngữ đa phương ngữ**: cân nhắc giọng miền Nam & Trung ở các phiên bản sau.
- **Đạo đức dữ liệu đề thi**: chỉ dùng nội dung công khai, ghi rõ nguồn, sẵn sàng gỡ nếu trường yêu cầu.
- **Áp lực tâm lý**: tuyệt đối không hiển thị "điểm số / xếp hạng" ở UI cho trẻ; chỉ phụ huynh xem dashboard.

---

## 8. Câu hỏi mở / hướng nghiên cứu tiếp theo (mới ở v2)

1. **Có cần module "Phụ huynh đồng hành" không?** — co-play vs. solo play cho trẻ 3–4 tuổi: cái nào hiệu quả hơn?
2. **Mức độ adapt difficulty bao nhiêu là đủ?** — Quá dễ → chán; quá khó → khóc → bỏ cuộc. Cần A/B test với 3 mức bước nhảy.
3. **Voice recognition tiếng Việt cho trẻ** — chính xác bao nhiêu phần trăm? Có thể dùng trong "Nhắc lại theo cô" (Tiếng Anh) và "Đọc to chữ này" (Tiếng Việt) không?
4. **Cộng tác với trường/trung tâm** — có nên build module B2B (cho GV mầm non sử dụng trên TV lớp) song song với app B2C cho phụ huynh?
5. **Phiên bản có người lớn cùng chơi vs. trẻ chơi 1 mình** — phù hợp 3–4 tuổi (cần kèm) vs. 5–6 tuổi (có thể tự chơi).
6. **Liệu game có thể giúp giảm áp lực phụ huynh** — đo bằng survey "Parental Anxiety about School Readiness" trước/sau 4 tuần dùng app?

---

## 9. Nguồn tham khảo chính

### 9.1 Tiền tiểu học & chính sách
- Tạp chí Giáo dục — Hướng dẫn tuyển sinh Hà Nội 2025–2026 (tapchigiaoduc.edu.vn)
- Giáo dục Việt Nam — Thi nhau cho học tiền tiểu học (giaoduc.net.vn)
- VOV2 — Lớp tiền tiểu học không quyết định kết quả học tập
- Luật Việt Nam — Thông tư 29/2024/TT-BGDĐT về dạy thêm – học thêm (luatvietnam.vn, vneconomy.vn)
- LuatNguyen — Luật Giáo dục sửa đổi 2025 số 123/2025/QH15, hiệu lực 01/01/2026
- IS HCMC, WASS, Sentia, Phenikaa, VAS — chương trình tiền tiểu học
- baochinhphu.vn, vtv.vn — Chương trình GDMN mới
- ischool.vn, idj.com.vn — 5 lĩnh vực phát triển trẻ mầm non (Thông tư 17 + VBHN 01/2021)
- hoatieu.vn — Văn bản hợp nhất số 01/2021/VBHN-BGDĐT

### 9.2 Trường & đề thi
- nsps.edu.vn, nguyensieu.edu.vn — Tuyển sinh Nguyễn Siêu (PDF Spark in Me 2026–2027)
- aschool.edu.vn — A/TEST 2025–2026, LET'S START 2026, A-Center
- situ.edu.vn, ngoisaohanoi.edu.vn — Ngôi Sao Hà Nội tuyển sinh 2026–2027
- vinschool.edu.vn — Tuyển sinh tiểu học 2026–2027 (Tư duy logic + phỏng vấn)
- mariecuriehanoischool.com — Quan điểm Marie Curie (không kiểm tra kiến thức)
- doanthidiem.edu.vn, tailieuonthi.io.vn — Bộ đề luyện 2026 ĐTĐ CLC & Cambridge
- znews.vn, victoryschoolbmt.edu.vn — Phân tích chi tiết 3 dạng IQ ĐTĐ
- vndoc.com — đề thi thử Archimedes Toán & Tiếng Việt
- mathx.vn, mathexpress.vn — Bộ tài liệu Ngôi Sao khối 1
- 123docz.net, lop5.net — đề thi mẫu PDF
- haysiri.com — Review thi & học Archimedes / Vinschool
- thanhnien.vn, tienphong.vn, znews.vn — Muôn kiểu tuyển lớp 1 trường tư Hà Nội
- afamily.vn, cafef.vn — Kinh nghiệm luyện thi từ phụ huynh
- lamchame.com — Kiểm tra đầu vào Vinschool Times City
- nhandan.vn, dantri.com.vn, congdankhuyenhoc.vn — Tổng hợp tuyển sinh trường tư 2025–2026
- tuyensinh.wellspring.edu.vn, event.theolympiaschools.edu.vn — Wellspring & Olympia
- gpaenglish.edu.vn — Học phí trường quốc tế HN

### 9.3 Cộng đồng & app
- contuhoc.com — Cộng đồng phụ huynh 175k thành viên + 3 group Facebook chính thức
- lamchame.com, webtretho.com, afamily.vn — Diễn đàn phụ huynh
- meviet.vn, oanhviela.com, medayroi.com, nuoicondung.com — Review KidsUp/Monkey Junior/VMonkey
- giaoducsomkidsup.com — VMonkey theo GDPT mới
- ucmasvietnam.com, lienviet.edu.vn, sentiaschool.edu.vn, edulive.net, kiomath.edu.vn — Trung tâm tiền tiểu học Hà Nội
- giasudantri.vn — TOP trung tâm tiền tiểu học HN
- dienmayxanh.com, classpoint.io — Top game giáo dục cho trẻ

### 9.4 Khung chuẩn giáo dục & UX
- Chương trình GDMN 2021 (Văn bản hợp nhất 01/2021/VBHN-BGDĐT) & Chuẩn phát triển trẻ 5 tuổi — Bộ GD&ĐT
- EYFS (UK) — gov.uk/government/publications/early-years-foundation-stage-framework
- Cambridge Early Years + ASPECTS + BASE — cambridge.org/insight
- Montessori, HighScope KDIs — khung đối chiếu quốc tế
- Nielsen Norman Group — UX Design for Children Ages 3–12 (4th Ed.); Designing for Kids: Cognitive Considerations; Children's UX: Usability Issues
- UXmatters — UX Design for Kids: Key Design Considerations
- Oxford Academic Interacting with Computers — Interactive Design Framework for Children's Apps for Enhancing Emotional Experience

### 9.5 Đánh giá game & nghiên cứu giáo dục VN
- VJE Tạp chí Giáo dục 2024 (tcgd.tapchigiaoduc.edu.vn) — Nghiên cứu game giáo dục theo TAM
- Tạp chí Khoa học ĐHSP TP HCM (journal.hcmue.edu.vn)
- ClassIn VN — Game-Based Assessment trong giáo dục 4.0
- DLU Scholar — Biện pháp giáo dục kỹ năng giải quyết vấn đề cho trẻ 5 tuổi
- MEEGA+ — Model for Evaluation of Educational Games

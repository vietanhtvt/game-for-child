# Schema & Cấu Trúc Lưu Trữ Dữ Liệu — Game Tiền Tiểu Học

_Cập nhật: 2026-05-02_

Tài liệu này mô tả toàn bộ hệ thống lưu trữ câu hỏi, tài sản media (hình ảnh, âm thanh) và index để phục vụ phát triển game giáo dục cho trẻ 3–6 tuổi. Được thiết kế để: (1) dễ truy xuất theo nhiều chiều, (2) hỗ trợ đầy đủ hình ảnh & âm thanh, (3) mở rộng được khi game phát triển.

---

## 1. Cây Thư Mục

```
data/
├── schema/                          # JSON Schema — định nghĩa cấu trúc dữ liệu
│   ├── question.schema.json         # Schema một câu hỏi (xem mục 3)
│   └── question-set.schema.json     # Schema tập câu hỏi (file .json trong questions/)
│
├── questions/                       # Câu hỏi phân loại theo trường
│   ├── archimedes/
│   │   ├── math-pattern.json        # Quy luật hình, đếm, hình học
│   │   └── spatial-number.json      # Không gian, số học
│   ├── nguyen-sieu/
│   │   └── storytelling.json        # Kể chuyện, trí nhớ, cảm xúc
│   ├── doan-thi-diem/
│   │   ├── iq-observation.json      # IQ quan sát, tìm điểm khác, trí nhớ
│   │   └── math-counting.json       # Đếm, so sánh, quy luật, cộng/trừ
│   ├── vinschool/
│   │   └── english-listening.json   # Tiếng Anh: từ vựng, nghe hiểu, chỉ dẫn
│   ├── ngoi-sao/
│   │   ├── math-counting.json       # Toán đếm, quy luật, hình học
│   │   └── vietnamese-letters.json  # Chữ cái, ghép vần, đọc hiểu
│   └── general/                     # Kỹ năng chung, không theo trường cụ thể
│       ├── vietnamese-letters.json  # Bảng chữ cái, ghép âm tiết
│       └── spatial-emotion.json     # Không gian + nhận diện cảm xúc
│
├── assets/                          # Media files (hình ảnh & âm thanh)
│   ├── images/
│   │   ├── questions/               # Hình ảnh đề bài (640×480 WebP)
│   │   ├── options/                 # Lựa chọn A/B/C/D (200×200 WebP)
│   │   ├── backgrounds/             # Nền mini-game (1280×720 WebP)
│   │   ├── characters/              # Nhân vật game
│   │   └── feedback/                # Hình phản hồi đúng/sai
│   ├── audio/
│   │   ├── questions/               # Audio đọc câu hỏi (MP3, 48kbps mono)
│   │   ├── instructions/            # Hướng dẫn chơi
│   │   ├── feedback/                # Âm phản hồi (đúng, thử lại)
│   │   └── background/              # Nhạc nền nhẹ nhàng
│   └── video/
│       └── stories/                 # Video hoạt hình truyện ngắn (MP4, <30s)
│
└── index/                           # Các file index để truy xuất nhanh
    ├── by-skill.json                # Theo kỹ năng → danh sách question_id
    ├── by-school.json               # Theo trường → danh sách question_id
    ├── by-difficulty.json           # Theo độ khó 1–4
    └── by-question-type.json        # Theo dạng bài (multiple-choice, counting…)
```

---

## 2. Lý Do Chọn Hình Thức Lưu Trữ

### 2.1 Tại sao JSON thay vì Database?

| Tiêu chí | JSON files | SQLite | PostgreSQL |
|---|---|---|---|
| Setup zero-config | ✅ | ✅ | ❌ |
| Version control (git diff) | ✅ | ❌ | ❌ |
| Hỗ trợ cấu trúc lồng nhau (media paths) | ✅ | Phức tạp | Phức tạp |
| Import vào game engine (Unity/Flutter) | ✅ trực tiếp | Cần driver | Cần driver |
| Tìm kiếm nhanh | Qua index/ | ✅ | ✅ |
| Scale khi > 10,000 câu hỏi | ❌ → migrate | ✅ | ✅ |

**Quyết định**: Dùng JSON + index files cho giai đoạn MVP (<500 câu). Khi đạt 500+ câu hỏi, migrate sang SQLite hoặc Supabase giữ nguyên schema.

### 2.2 Tại sao WebP cho hình ảnh?

- Nhỏ hơn PNG 25–35%, nhỏ hơn JPEG khi cần transparency
- Hỗ trợ tốt trên iOS 14+, Android 4.0+, tất cả browser hiện đại
- Fallback: PNG (giữ cả hai nếu target iOS cũ hơn)
- SVG cho icon đơn giản (hình học, chữ cái) — scalable không mờ

### 2.3 Tại sao MP3 cho âm thanh?

- Hỗ trợ phổ quát nhất trên mọi nền tảng
- 48kbps mono đủ chất lượng giọng nói — file ~200KB cho clip 30s
- OGG làm fallback cho web (Firefox)
- WAV chỉ dùng cho âm thanh cần chỉnh sửa (raw master)

---

## 3. Schema Câu Hỏi (question.schema.json)

Mỗi câu hỏi là một JSON object. Các trường bắt buộc: `id`, `type`, `skill`, `difficulty`, `stem`.

### 3.1 Trường cốt lõi

```json
{
  "id": "ARCH-PATT-0001",
  "type": "pattern-next",
  "skill": "pattern-recognition",
  "difficulty": 2,
  "school": ["archimedes", "general"],
  "source": {
    "origin": "vndoc.com",
    "url": "https://vndoc.com/...",
    "crawled_at": "2026-04-20",
    "verified": false
  }
}
```

### 3.2 Nội dung câu hỏi (ContentBlock — hỗ trợ đa phương tiện)

```json
"stem": {
  "text": "Hình nào đến tiếp theo trong dãy?",
  "audio": {
    "path": "questions/arch-patt-0001-stem.mp3",
    "locale": "vi-VN",
    "speaker": "female-child-north",
    "duration_ms": 2100
  },
  "image": {
    "path": "questions/arch-patt-0001-sequence.webp",
    "alt": "Dãy hình: tròn đỏ, vuông xanh, tròn đỏ, vuông xanh, dấu hỏi",
    "width": 600,
    "height": 120,
    "license": "original"
  }
}
```

`ContentBlock` xuất hiện ở: `stem`, `hint`, `feedback.correct`, `feedback.incorrect`, `feedback.explanation`.

### 3.3 Lựa chọn (Options)

```json
"options": [
  {
    "id": "A",
    "image": { "path": "options/arch-patt-0001-A.webp", "alt": "Hình tròn đỏ" },
    "audio": { "path": "options/arch-patt-0001-A.mp3", "locale": "vi-VN" }
  },
  { "id": "B", "image": { "path": "options/arch-patt-0001-B.webp", "alt": "Hình vuông xanh" } },
  { "id": "C", "text": "7" }
]
```

- `id` hợp lệ: `A`, `B`, `C`, `D` hoặc số `1`–`6`
- Mỗi option có thể có `text`, `image`, `audio` độc lập hoặc kết hợp
- Câu hỏi Tiếng Anh: option thường chỉ có `image` + `audio` (tiếng Anh bản ngữ)

### 3.4 Đáp án (Answer — 3 dạng)

```json
// Single answer
"answer": "C"

// Multiple / sequence answer
"answer": ["1", "4", "2", "3"]

// Matching (key-value pairs)
"answer": {
  "left-A": "right-2",
  "left-B": "right-3",
  "left-C": "right-1"
}
```

### 3.5 Cấu hình game (game_config)

```json
"game_config": {
  "mini_game": "doan-tau-hoa-tiet",
  "time_limit_seconds": 60,
  "max_attempts": 3,
  "auto_play_audio": true,
  "shuffle_options": true,
  "background_audio": {
    "path": "background/playful-light-01.mp3"
  }
}
```

---

## 4. Dạng Câu Hỏi (Question Types)

| Type | Mô tả | Mini-game | Ví dụ |
|---|---|---|---|
| `multiple-choice-image` | Chọn 1 trong 4 hình | `hai-buc-tranh`, `bat-chu` | Tìm hình khác biệt |
| `multiple-choice` | Chọn 1 trong 4 văn bản/số | `cho-que` | Chọn số đếm đúng |
| `pattern-next` | Hoàn thiện quy luật dãy hình | `doan-tau-hoa-tiet` | AB-AB → ? |
| `counting` | Đếm vật và chọn số | `cho-que` | Đếm 7 con bướm |
| `spot-difference` | Tìm điểm khác giữa 2 tranh | `hai-buc-tranh` | Tìm 3 điểm khác |
| `story-arrange` | Sắp xếp tranh kể chuyện | `ke-chuyen` | 4 tranh → câu chuyện |
| `matching` | Nối đôi (con vật - nhà) | `ke-chuyen` | Chim → tổ |
| `listen-choose` | Nghe âm/từ → chọn hình | `touch-and-say` | Nghe "cat" → chọn con mèo |
| `sequence` | Sắp xếp thứ tự (to→nhỏ, 1→10) | `lat-the-doi` | Xếp theo thứ tự |
| `fill-blank` | Điền vào chỗ trống (ghép vần) | `ghep-van` | C + A + sắc = ? |
| `connect-dots` | Nối các chấm thành hình | `to-net` | Nối 10 chấm → ngôi sao |
| `coloring` | Tô màu theo hướng dẫn | `to-net` | Tô hình tròn màu xanh |
| `drag-drop` | Kéo thả hình vào đúng vị trí | `xep-tangram` | Ghép Tangram |

---

## 5. Kỹ Năng (Skills)

| Kỹ năng | Mô tả | Trường tham chiếu |
|---|---|---|
| `letter-recognition` | Nhận diện 29 chữ cái in hoa/thường | Archimedes, ĐTĐ |
| `syllable-blending` | Ghép phụ âm + nguyên âm + thanh | Archimedes, Ngôi Sao |
| `word-reading` | Đọc từ đơn giản | Vinschool, ĐTĐ |
| `reading-comprehension` | Nghe/đọc đoạn ngắn → trả lời | Nguyễn Siêu, ĐTĐ |
| `storytelling` | Kể chuyện / sắp xếp tranh | Nguyễn Siêu, ĐTĐ |
| `counting` | Đếm 1–20 | Mọi trường |
| `number-comparison` | Nhiều hơn/ít hơn, lớn hơn/nhỏ hơn | Mọi trường |
| `addition` | Phép cộng trong phạm vi 10–20 | ĐTĐ, Ngôi Sao |
| `subtraction` | Phép trừ trong phạm vi 10 | ĐTĐ, Vinschool |
| `pattern-recognition` | Quy luật hình/dãy số | Archimedes, Nguyễn Siêu |
| `geometry` | Nhận diện hình: tròn, vuông, tam giác… | Mọi trường |
| `spatial-orientation` | Trên/dưới, trái/phải, trong/ngoài | Chung |
| `dot-connection` | Nối chấm → chuẩn bị viết | ĐTĐ |
| `iq-observation` | Tìm hình khác biệt, quan sát | ĐTĐ, Nguyễn Siêu |
| `iq-memory` | Trí nhớ làm việc, lật thẻ đôi | Nguyễn Siêu |
| `iq-logic` | Nối đôi theo logic, nhân quả | ĐTĐ, Archimedes |
| `english-vocabulary` | Từ vựng cơ bản tiếng Anh | Vinschool, Archimedes |
| `english-listening` | Nghe hiểu câu đơn giản | Vinschool, Nguyễn Siêu |
| `english-following-instructions` | Làm theo lệnh tiếng Anh | Vinschool Cambridge |
| `fine-motor` | Vận động tinh: tô, nối, kéo thả | ĐTĐ |
| `emotion-recognition` | Nhận diện cảm xúc từ gương mặt | Chung (EQ) |
| `social-skills` | Hợp tác, chia sẻ, chờ đến lượt | Chung |

---

## 6. Hệ Thống Độ Khó

| Level | Tuổi | Đặc điểm | Ví dụ |
|---|---|---|---|
| **1** | 3 tuổi | Nhận biết cơ bản: màu, hình, số 1–5, chữ đơn | Tô màu 1 hình tròn; đếm 3 táo |
| **2** | 4 tuổi | Đếm 1–10, âm đơn, quy luật AB-AB, cảm xúc | Đếm 7 bướm; tìm hình tròn trong hàng |
| **3** | 5 tuổi | Quy luật phức tạp, ghép vần, IQ cơ bản, Anh văn cơ bản | Dãy hình ABC-ABC; nghe "cat" → chọn hình |
| **4** | 6 tuổi | Kể chuyện, working memory, IQ nâng cao, chỉ dẫn Anh | Sắp xếp 4 tranh; tìm vật bị giấu; "touch the red ball" |

---

## 7. Quy Ước Đặt Tên ID

```
{SCHOOL_CODE}-{SKILL_CODE}-{INDEX4}
```

| School code | Trường |
|---|---|
| `ARCH` | Archimedes Academy / School |
| `NSPS` | Nguyễn Siêu (Spark in Me) |
| `DTD` | Đoàn Thị Điểm (chuẩn & Cambridge) |
| `VIN` | Vinschool (chuẩn & Cambridge) |
| `NGS` | Ngôi Sao Hà Nội |
| `GEN` | General (không theo trường cụ thể) |

| Skill code | Kỹ năng |
|---|---|
| `LETT` | letter-recognition |
| `SYLL` | syllable-blending |
| `READ` | reading-comprehension |
| `STOR` | storytelling |
| `PATT` | pattern-recognition |
| `COUNT` | counting |
| `NUM` | number-comparison |
| `ADD` | addition |
| `GEO` | geometry |
| `SPAT` | spatial-orientation |
| `IQ` | iq-observation |
| `MEM` | iq-memory |
| `LOG` | iq-logic |
| `ENG` | english-vocabulary / english-listening |
| `EMO` | emotion-recognition |
| `EMOQ` | emotion-recognition (question variant) |
| `FINE` | fine-motor |

---

## 8. Quy Ước Đặt Tên Asset

### 8.1 Hình ảnh

```
data/assets/images/
  questions/{id-lowercase}-{role}.{ext}
  options/{id-lowercase}-{option_id}.{ext}
  backgrounds/{mini_game}-{variant}.webp
  characters/{char_name}-{pose}.webp
  feedback/{type}-{index}.webp
```

Ví dụ:
- `questions/arch-patt-0001-sequence.webp` — hình dãy quy luật
- `questions/dtd-iq-0003-left.webp` — ảnh bên trái của cặp "tìm điểm khác"
- `options/arch-patt-0001-A.webp` — lựa chọn A của câu ARCH-PATT-0001
- `backgrounds/doan-tau-hoa-tiet-day.webp` — nền mini-game đoàn tàu

| Loại | Kích thước chuẩn | Format |
|---|---|---|
| Hình câu hỏi | 640×480 | WebP (PNG fallback) |
| Hình lựa chọn | 200×200 | WebP |
| Nền mini-game | 1280×720 | WebP |
| Icon chữ cái / số | 80×80 | SVG ưu tiên |
| Nhân vật | Theo thiết kế | SVG hoặc WebP |

### 8.2 Âm thanh

```
data/assets/audio/
  questions/{id-lowercase}-{role}.mp3
  options/{id-lowercase}-{option_id}-{locale}.mp3
  feedback/{type}-{index}.mp3
  instructions/{mini_game}-{locale}.mp3
  background/{mood}-{index}.mp3
```

Ví dụ:
- `questions/arch-patt-0001-stem.mp3` — audio đọc đề câu hỏi
- `questions/arch-patt-0002-hint.mp3` — gợi ý sau lần sai thứ 2
- `options/vin-eng-0001-A-en-US.mp3` — giọng English native cho option A
- `feedback/correct-generic-01.mp3` — âm khen ngợi chung
- `feedback/try-again-02.mp3` — "Thử lại nào!"
- `background/playful-light-01.mp3` — nhạc nền nhẹ nhàng

| Loại | Bitrate | Locale | Ghi chú |
|---|---|---|---|
| Giọng đọc đề bài | 48kbps mono | `vi-VN` | Giọng Bắc chuẩn, diễn cảm |
| Giọng hướng dẫn | 48kbps mono | `vi-VN` | Rõ ràng, chậm |
| Giọng tiếng Anh | 64kbps mono | `en-US` | Native speaker (không dùng TTS robot) |
| Nhạc nền | 96kbps stereo | — | Nhẹ, không gây phân tâm |
| Âm phản hồi (SFX) | 48kbps | — | Vui tươi, không gây hoảng |

---

## 9. File Index — Truy Xuất Nhanh

### 9.1 by-skill.json
```json
{
  "pattern-recognition": {
    "label": "Quy luật hình / Dãy hình",
    "mini_games": ["doan-tau-hoa-tiet"],
    "questions": [
      { "id": "ARCH-PATT-0001", "file": "questions/archimedes/math-pattern.json", "difficulty": 2 }
    ]
  }
}
```

### 9.2 by-school.json
```json
{
  "archimedes": {
    "label": "Archimedes Academy",
    "questions": ["ARCH-PATT-0001", "ARCH-PATT-0002", ...]
  }
}
```

### 9.3 by-difficulty.json
```json
{
  "1": { "label": "3 tuổi", "questions": [...] },
  "2": { "label": "4 tuổi", "questions": [...] },
  "3": { "label": "5 tuổi", "questions": [...] },
  "4": { "label": "6 tuổi", "questions": [...] }
}
```

### 9.4 by-question-type.json
```json
{
  "pattern-next": {
    "mini_games": ["doan-tau-hoa-tiet"],
    "questions": [...]
  }
}
```

---

## 10. Số Lượng Câu Hỏi Hiện Tại (2026-05-02)

| Trường | File | Số câu | Kỹ năng |
|---|---|---|---|
| Archimedes | math-pattern.json | 5 | pattern, counting, geometry |
| Archimedes | spatial-number.json | ~5 | spatial, number-comparison, addition |
| Đoàn Thị Điểm | iq-observation.json | 6 | iq-observation, storytelling, iq-memory, iq-logic |
| Đoàn Thị Điểm | math-counting.json | ~5 | counting, number-comparison, pattern, addition |
| Vinschool | english-listening.json | 5 | english-vocabulary, english-listening, english-instructions |
| Nguyễn Siêu | storytelling.json | ~5 | storytelling, reading-comprehension, iq-memory, emotion |
| Ngôi Sao | math-counting.json | 6 | counting, number-comparison, pattern, geometry |
| Ngôi Sao | vietnamese-letters.json | 5 | letter-recognition, syllable-blending, reading-comprehension |
| General | vietnamese-letters.json | ~6 | letter-recognition, syllable-blending, reading-comprehension |
| General | spatial-emotion.json | ~6 | spatial-orientation, emotion-recognition |
| **Tổng** | | **~54 câu** | |

**Mục tiêu theo giai đoạn:**
- MVP (6 mini-game): **150 câu** (25 câu/kỹ năng × 6 kỹ năng)
- Beta: **300 câu** (thêm Tiếng Anh và kể chuyện)
- v1.0: **500+ câu** (đủ để adaptive difficulty có ý nghĩa)

---

## 11. Quy Trình Thêm Câu Hỏi Mới

```
1. Chọn file đúng theo trường & kỹ năng
   → Nếu chưa có file: tạo {school}/{skill-slug}.json mới

2. Đặt ID theo quy ước, tiếp nối số thứ tự cao nhất hiện có
   Ví dụ: ARCH-PATT-0005 đã có → câu mới là ARCH-PATT-0006

3. Điền đầy đủ các trường bắt buộc: id, type, skill, difficulty, stem

4. Ghi rõ source.origin và source.verified = false (nếu từ nguồn bên ngoài)

5. Đặt path placeholder cho image/audio — dùng quy ước đặt tên ở mục 8
   alt text bằng tiếng Việt mô tả chính xác hình ảnh (accessibility)

6. Cập nhật tất cả các file index liên quan:
   - data/index/by-skill.json
   - data/index/by-school.json
   - data/index/by-difficulty.json
   - data/index/by-question-type.json

7. Cập nhật total_questions trong file .json chứa câu hỏi đó
```

---

## 12. Nguồn Gốc & Bản Quyền

Mọi câu hỏi đều ghi rõ `source.origin`. Các giá trị hợp lệ:

| origin | Ý nghĩa |
|---|---|
| `vndoc.com` | Crawl từ vndoc.com |
| `tailieuonthi.io.vn` | Crawl từ tailieuonthi.io.vn |
| `lop5.net` | Crawl từ lop5.net |
| `123docz.net` | Crawl từ 123docz.net / PDF 123doc |
| `mathx.vn` | Crawl từ mathx.vn |
| `mathexpress.vn` | Crawl từ mathexpress.vn |
| `khotientieuhoc.com` | Crawl từ khotientieuhoc.com |
| `bevaolop1.com` | Crawl từ bevaolop1.com |
| `giaovienvietnam.com` | Crawl từ giaovienvietnam.com |
| `download.vn` | Crawl từ download.vn |
| `haysiri.com` | Crawl từ haysiri.com |
| `afamily.vn` | Crawl từ afamily.vn |
| `lamchame.com` | Crawl từ lamchame.com |
| `contuhoc.com` | Crawl từ contuhoc.com |
| `official-school` | Từ website chính thức của trường |
| `original` | Tự tạo — không phụ thuộc bản quyền |

**Nguyên tắc:**
- `verified: false` — câu hỏi crawl chưa được giáo viên kiểm tra
- `verified: true` — đã được chuyên gia/giáo viên xác nhận tính sư phạm và độ chính xác
- Nếu trường yêu cầu gỡ nội dung → xóa và thay bằng câu `original`
- Mục tiêu dài hạn: ≥ 70% câu hỏi có `origin: "original"` và `verified: true`

---

## 13. Hướng Phát Triển Schema Sau MVP

### 13.1 Adaptive Difficulty
Thêm trường `irt_difficulty` (Item Response Theory) sau khi có dữ liệu playtest:
```json
"metadata": {
  "irt_difficulty": 0.72,
  "avg_correct_rate": 0.63,
  "avg_time_seconds": 18
}
```

### 13.2 Localization
Thêm `text_en` vào `ContentBlock` cho phiên bản song ngữ:
```json
"stem": {
  "text": "Hình nào đến tiếp theo?",
  "text_en": "Which shape comes next?"
}
```

### 13.3 Multi-region Audio
Thêm locale `vi-VN-south` khi mở rộng ra miền Nam:
```json
"audio": {
  "path": "questions/arch-patt-0001-stem-south.mp3",
  "locale": "vi-VN-south",
  "speaker": "female-child-south"
}
```

### 13.4 Migration sang Database
Khi đạt 500+ câu, migrate sang SQLite (offline-first) hoặc Supabase (cloud):
- Schema JSON ánh xạ 1:1 vào bảng `questions`
- Assets tiếp tục lưu trên CDN / object storage (S3-compatible)
- Index files thay bằng SQL query hoặc full-text search

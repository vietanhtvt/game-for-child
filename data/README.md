# Thư Mục `data/` — Cấu Trúc & Hướng Dẫn

## Tổng quan

```
data/
├── schema/                    # JSON Schema definitions
│   ├── question.schema.json   # Schema cho một câu hỏi
│   └── question-set.schema.json # Schema cho tập câu hỏi
│
├── questions/                 # Câu hỏi phân theo trường
│   ├── archimedes/            # Archimedes Academy (AAPS/ADAS)
│   ├── nguyen-sieu/           # Tiểu học Nguyễn Siêu (Spark in Me)
│   ├── doan-thi-diem/         # Đoàn Thị Điểm (chuẩn & Cambridge)
│   ├── vinschool/             # Vinschool (chuẩn & Cambridge)
│   ├── ngoi-sao/              # Ngôi Sao Hà Nội
│   └── general/               # Kỹ năng chung, không theo trường cụ thể
│
├── assets/                    # Media files (chưa có — cần tạo/mua)
│   ├── images/
│   │   ├── questions/         # Hình ảnh câu hỏi (640×480 WebP max)
│   │   ├── options/           # Hình ảnh lựa chọn A/B/C/D (200×200 WebP)
│   │   ├── backgrounds/       # Nền mini-game
│   │   ├── characters/        # Nhân vật game
│   │   └── feedback/          # Hình phản hồi đúng/sai
│   └── audio/
│       ├── questions/         # Audio đọc câu hỏi (MP3, ~48kbps mono)
│       ├── instructions/      # Hướng dẫn chơi
│       ├── feedback/          # Âm phản hồi (correct, try-again)
│       └── background/        # Nhạc nền nhẹ nhàng
│
└── index/                     # Index để truy xuất nhanh
    ├── by-skill.json          # Theo kỹ năng
    ├── by-school.json         # Theo trường
    ├── by-difficulty.json     # Theo độ khó (1-4)
    └── by-question-type.json  # Theo dạng bài
```

## Dạng câu hỏi (Question Types)

| Type | Mô tả | Mini-game |
|---|---|---|
| `multiple-choice-image` | Chọn 1 trong 4 hình | bat-chu, hai-buc-tranh |
| `multiple-choice` | Chọn 1 trong 4 văn bản | bat-chu |
| `pattern-next` | Hoàn thiện quy luật dãy hình | doan-tau-hoa-tiet |
| `counting` | Đếm vật và chọn số | cho-que |
| `spot-difference` | Tìm điểm khác giữa 2 tranh | hai-buc-tranh |
| `story-arrange` | Sắp xếp tranh kể chuyện | ke-chuyen |
| `matching` | Nối đôi (con vật - nhà) | ke-chuyen |
| `listen-choose` | Nghe âm → chọn hình | touch-and-say |
| `sequence` | Sắp xếp thứ tự | lat-the-doi |
| `fill-blank` | Điền vào chỗ trống | ghep-van |
| `connect-dots` | Nối các chấm thành hình | to-net |
| `coloring` | Tô màu theo hướng dẫn | to-net |
| `drag-drop` | Kéo thả hình vào đúng chỗ | xep-tangram |

## Quy ước đặt tên ID câu hỏi

```
{SCHOOL_CODE}-{SKILL_CODE}-{INDEX4}
```

| School code | Trường |
|---|---|
| `ARCH` | Archimedes |
| `NSPS` | Nguyễn Siêu |
| `DTD` | Đoàn Thị Điểm |
| `VIN` | Vinschool |
| `NGS` | Ngôi Sao |
| `GEN` | General |

| Skill code | Kỹ năng |
|---|---|
| `LETT` | letter-recognition |
| `SYLL` | syllable-blending |
| `READ` | reading-comprehension |
| `STOR` | storytelling |
| `PATT` | pattern-recognition |
| `COUNT` | counting |
| `GEO` | geometry |
| `IQ` | iq-observation |
| `MEM` | iq-memory |
| `LOG` | iq-logic |
| `ENG` | english-vocabulary/listening |
| `EMO` | emotion-recognition |
| `EMOQ` | emotion-recognition (question) |

## Quy ước đặt tên Asset

### Hình ảnh
- Format: **WebP** (ưu tiên), PNG fallback
- Câu hỏi: `questions/{id-lowercase}-{role}.webp` — VD: `arch-patt-0001-sequence.webp`
- Lựa chọn: `options/{id-lowercase}-{option_id}.webp` — VD: `arch-patt-0001-A.webp`
- Kích thước chuẩn: câu hỏi 640×480, lựa chọn 200×200, nền 1280×720

### Âm thanh
- Format: **MP3** (48kbps mono cho tiếng nói), OGG fallback
- Tên file theo ID câu hỏi + role: `{id-lowercase}-stem.mp3`, `{id-lowercase}-hint.mp3`
- Giọng đọc: `vi-VN` giọng Bắc chuẩn, `en-US` native speaker
- Không dùng TTS robot — dùng voice actor hoặc TTS chất lượng cao (Google WaveNet / Azure Neural)

## Độ khó

| Level | Tuổi | Mô tả |
|---|---|---|
| 1 | 3 tuổi | Nhận biết cơ bản: màu, hình, số 1-5, chữ đơn |
| 2 | 4 tuổi | Đếm 1-10, âm đơn, quy luật đơn giản, cảm xúc |
| 3 | 5 tuổi | Quy luật phức tạp, ghép vần nghe-chọn, IQ cơ bản, Anh văn cơ bản |
| 4 | 6 tuổi | Kể chuyện, trí nhớ làm việc, IQ nâng cao, Anh văn chỉ dẫn |

## Thêm câu hỏi mới

1. Chọn đúng file theo trường (hoặc `general/` nếu không theo trường)
2. Tạo file mới nếu chủ đề hoàn toàn mới: `{school}/{skill-slug}.json`
3. Đặt ID theo quy ước, tiếp nối số thứ tự hiện có
4. Thêm entry vào tất cả các file index liên quan
5. Đặt placeholder cho path asset, ghi rõ `alt` text
6. Đặt `source.origin` và `source.verified: false` cho câu hỏi crawl

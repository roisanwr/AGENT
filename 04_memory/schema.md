# Skema Memory — Google Sheets

> Dokumen ini mendefinisikan struktur data Google Sheets yang digunakan sebagai persistent memory system untuk AGENT.

---

## Arsitektur Sheets

Gunakan **satu Google Spreadsheet** dengan beberapa sheet (tab):

```
AGENT_Memory.gsheet
├── Sheet 1: users         → Profil dan preferensi user
├── Sheet 2: history       → Riwayat prompt yang dibuat
├── Sheet 3: model_kb      → Knowledge base model (future use)
└── Sheet 4: feedback      → Rating dan feedback user
```

---

## Sheet 1: `users`

Sheet ini menyimpan profil dan preferensi user.

### Struktur Kolom

| Kolom | Nama | Tipe | Deskripsi |
|-------|------|------|-----------|
| A | `user_id` | String | ID unik user (bisa email atau generated ID) |
| B | `display_name` | String | Nama yang dipanggil oleh agent |
| C | `created_at` | DateTime | Kapan user pertama kali menggunakan sistem |
| D | `last_active` | DateTime | Kapan terakhir aktif |
| E | `favorite_category` | String | Kategori yang paling sering digunakan |
| F | `favorite_image_model` | String | Model gambar favorit |
| G | `favorite_video_model` | String | Model video favorit |
| H | `favorite_audio_model` | String | Model audio favorit |
| I | `favorite_text_model` | String | Model teks favorit |
| J | `preferred_image_style` | String | Style gambar yang sering diminta |
| K | `preferred_image_ratio` | String | Aspect ratio yang paling sering dipakai |
| L | `preferred_content_tone` | String | Tone penulisan yang disukai |
| M | `preferred_language` | String | Bahasa output yang diinginkan |
| N | `notes` | String | Catatan bebas tentang preferensi user |

### Contoh Baris Data

```
A: user_001
B: Rois
C: 2026-05-01 09:00:00
D: 2026-05-20 14:30:00
E: gambar
F: midjourney
G: veo
H: suno
I: claude
J: dark fantasy, cinematic
K: 16:9
L: conversational, warm
M: Bahasa Indonesia
N: suka karakter dengan detail tinggi, sering minta niji mode untuk anime
```

---

## Sheet 2: `history`

Sheet ini menyimpan riwayat semua prompt yang dibuat.

### Struktur Kolom

| Kolom | Nama | Tipe | Deskripsi |
|-------|------|------|-----------|
| A | `session_id` | String | ID unik sesi ini |
| B | `user_id` | String | Foreign key ke tabel users |
| C | `timestamp` | DateTime | Kapan prompt dibuat |
| D | `category` | String | gambar/video/audio/coding/persona/konten |
| E | `target_model` | String | Model yang dipilih |
| F | `input_summary` | String | Ringkasan input user (max 200 karakter) |
| G | `prompt_output` | String | Prompt yang dihasilkan |
| H | `user_rating` | Integer | Rating user: 1-5 (opsional) |
| I | `revision_count` | Integer | Berapa kali direvisi sebelum final |
| J | `tags` | String | Tag tambahan (comma-separated) |

### Contoh Baris Data

```
A: sess_20260520_001
B: user_001
C: 2026-05-20 14:30:00
D: gambar
E: midjourney
F: dark fantasy warrior woman for YouTube thumbnail
G: A fierce female warrior with silver hair... --ar 16:9 --style raw --s 500
H: 5
I: 1
J: fantasy, warrior, dark, thumbnail
```

---

## Sheet 3: `model_kb` (Future Use)

Sheet ini direncanakan untuk menyimpan knowledge model secara dinamis (saat ini sudah ada di Markdown files, ini untuk auto-sync di masa depan).

### Struktur Kolom (Planned)

| Kolom | Nama | Deskripsi |
|-------|------|-----------|
| A | `model_id` | ID model |
| B | `model_name` | Nama model |
| C | `category` | Kategori |
| D | `version` | Versi model |
| E | `last_updated` | Kapan terakhir diperbarui |
| F | `key_syntax` | Parameter/sintaks utama |
| G | `best_for` | Keunggulan utama |
| H | `avoid` | Yang harus dihindari |

---

## Sheet 4: `feedback`

Sheet untuk mencatat feedback dan improvement ideas.

### Struktur Kolom

| Kolom | Nama | Tipe | Deskripsi |
|-------|------|------|-----------|
| A | `feedback_id` | String | ID unik feedback |
| B | `user_id` | String | Dari siapa |
| C | `timestamp` | DateTime | Kapan |
| D | `category` | String | Kategori yang dimaksud |
| E | `issue_type` | String | routing_error/bad_output/ux/feature_request |
| F | `description` | String | Deskripsi masalah atau saran |
| G | `status` | String | open/in_progress/resolved |
| H | `resolution` | String | Bagaimana diselesaikan |

---

## Cara Agent Membaca Memory

### Saat Awal Sesi (Step 2 — Classifier)
```
1. Ambil user_id dari session
2. Query sheet "users" berdasarkan user_id
3. Jika ditemukan: baca favorite_category, favorite_[kategori]_model, preferred_*
4. Gunakan untuk: mempersingkat klarifikasi, personalisasi rekomendasi
5. Jika tidak ditemukan: buat baris baru dengan created_at = sekarang
```

### Saat Akhir Sesi (Step 9 — Output)
```
1. Update last_active di sheet "users"
2. Update favorite_category (hitung frekuensi dari history)
3. Update favorite_model untuk kategori yang baru digunakan
4. Tambah baris baru ke sheet "history"
5. Update preferred_* jika user menyebutkan preferensi secara eksplisit
```

---

## Contoh Query Opal (Pseudocode)

**Membaca preferensi:**
```
GET users WHERE user_id = {current_user_id} LIMIT 1
→ Ambil kolom: favorite_category, favorite_image_model, preferred_image_style
```

**Menyimpan histori:**
```
INSERT INTO history:
  session_id: {generated_id}
  user_id: {current_user_id}
  timestamp: {now}
  category: {routing_result}
  target_model: {model_selected}
  input_summary: {first_150_chars_of_input}
  prompt_output: {final_prompt}
```

---

## Naming Convention

| Objek | Format | Contoh |
|-------|--------|--------|
| File Google Sheets | `AGENT_Memory` | - |
| User ID | `user_[timestamp]_[random3]` | `user_20260501_x7k` |
| Session ID | `sess_[YYYYMMDD]_[sequence]` | `sess_20260520_001` |
| Feedback ID | `fb_[YYYYMMDD]_[sequence]` | `fb_20260520_003` |

---

## Kebijakan Privasi & Data

- Data user disimpan di Google Sheets milik pemilik sistem
- User bisa minta penghapusan data dengan menghubungi admin
- History prompt tidak digunakan untuk melatih model
- Password atau data sensitif tidak boleh disimpan di sini

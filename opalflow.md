# Google Opal — Panduan Lengkap & Advanced Builder Manual

> Versi dokumen: 2.0 | Diperbarui: Mei 2026 | Berdasarkan riset dari blog.google, opaltool.com, datacamp, infoworld, venturebeat, dan komunitas Reddit

---

## BAGIAN 1 — ANATOMI OPAL (Fitur Tervalidasi 2026)

### 1.1 Apa Itu Google Opal?

Opal adalah **no-code AI workflow builder** dari Google Labs (rilis Juli 2025, update besar Maret 2026). Cara kerjanya: kamu mendeskripsikan apa yang mau dibuat dalam bahasa natural, Opal otomatis bikin **visual graph** dari node-node yang saling terhubung. Setiap node punya fungsi spesifik dan bisa di-edit manual lewat visual editor atau lewat perintah natural language tambahan.

**Penting:** Opal bukan tool coding. Opal adalah **AI orchestration platform** — kamu sutradara, Gemini yang mengeksekusi.

**URL akses:** `opal.withgoogle.com` atau lewat Gemini interface  
**Syarat:** Akun Google, tersedia di 160+ negara

---

### 1.2 Enam Node Utama (Building Blocks)

#### 🗃️ NODE 1: User Input

Titik masuk data dari pengguna. Tanpa node ini, workflow tidak bisa interaktif.

**Konfigurasi yang tersedia:**
| Setting | Fungsi |
|---------|--------|
| **Input Type** | Pilih: `Text` / `File Upload` / `Image` / `URL` |
| **Required / Optional** | Tentukan apakah wajib diisi |
| **Placeholder & Label** | Teks panduan yang tampil ke user |
| **Multi-input** | Bisa tambah beberapa field sekaligus |

**Cara user pakai `@` di kolom input:**
- `@` → buka menu: pilih file dari Google Drive atau upload lokal
- File yang dipilih akan jadi konteks untuk Generate node berikutnya

**Tips builder:**
- Beri label yang sangat deskriptif — ini yang user lihat
- Untuk AGENT project: satu Input node dengan field `tujuan` (required) + `konteks` (optional) + `target_model` (optional)

---

#### ✨ NODE 2: Generate (Otak AI)

Node terpenting. Di sini kamu define logika AI menggunakan system prompt + referensi data.

**Dua mode Generate:**

**Mode A — Fixed (Standard Generate):**
- Kamu tentukan model secara manual
- Alur deterministik dan predictable
- Cocok untuk: output yang harus konsisten formatnya

**Mode B — Agent (Agentic Generate):** *(ditambahkan Maret 2026)*
- AI memilih model dan tools sendiri secara otomatis
- Bisa tanya balik user (Interactive Chat) jika info kurang
- Bisa loop dan self-correct
- Cocok untuk: task yang open-ended dan membutuhkan judgment

**Model & Agent yang Tersedia di Opal (Mei 2026):**

Di dalam menu dropdown builder Opal, terdapat beberapa pilihan model/agent:
1. **Gemini 3 Flash** — Model default, terbaik untuk task sehari-hari (cepat dan efisien).
2. **Gemini 3.1 Pro** — Model penalaran tinggi, terbaik untuk task kompleks, coding, dan analisis mendalam.
3. **Nano Banana / Nano Banana Pro** — Model internal Opal untuk pembuatan/editing gambar.
4. **AudioLM / Lyria 2** — Model internal Opal untuk pembuatan musik & speech.
5. **Veo** — Model internal Opal untuk pembuatan video.

> [!IMPORTANT]
> **Scope Project AGENT:** Karena aplikasi kita murni berfokus pada **Prompt Engineering (Prompt Builder)** yang menghasilkan output berupa teks (prompt siap pakai), kita **hanya akan menggunakan model Gemini 3 (Flash & Pro)** untuk menjalankan seluruh logika workflow kita di Opal. Kita tidak menggunakan model gambar/video/audio internal Opal untuk memproses workflow ini.


**Konfigurasi System Prompt di Generate:**
```
[Isi system prompt langsung di text area Generate node]

Bisa pakai @ untuk referensi:
- @NamaAsset     → panggil file dari Assets
- @NamaStep      → ambil output dari step lain
- @tools.search  → aktifkan web search
- @tools.memory  → aktifkan memory lintas sesi
```

---

#### ⚙️ NODE 3: Tools (Kemampuan Eksekusi)

Tools dipanggil dari dalam Generate node menggunakan `@`. Berikut semua tools yang tersedia:

| Tool | Cara Panggil | Fungsi |
|------|-------------|--------|
| **Search Web** | `@tools.searchWeb` | Googling real-time untuk info terbaru |
| **Get Webpage** | `@tools.getWebpage` | Baca & ekstrak isi teks dari URL tertentu |
| **Get Weather** | `@tools.getWeather` | Data cuaca real-time lokasi tertentu |
| **Search Maps** | `@tools.searchMaps` | Data lokasi, rute, tempat dari Google Maps |
| **Code Execution** | `@tools.codeExecution` | Sandbox Python untuk kalkulasi/data processing |
| **Go to... (Router)** | `@tools.goto` | Dynamic routing — lompat ke node lain |
| **Use Memory** | `@tools.memory` | Baca/tulis memory lintas sesi |

**Cara mengaktifkan tools di System Prompt:**
```
Kamu adalah [persona].
Gunakan @tools.searchWeb jika butuh data terbaru.
Gunakan @tools.memory untuk baca preferensi user.

Setelah analisis, arahkan ke langkah berikutnya:
- Jika kategori = gambar: @tools.goto → [Node_Gambar]
- Jika kategori = audio: @tools.goto → [Node_Audio]
```

---

#### 🔀 NODE 4: Dynamic Routing (Go To)

Cara Opal menjalankan logika IF/THEN tanpa koding. Ini yang memungkinkan cabang-cabang di workflow.

**Cara kerja:**
1. Di dalam Generate node, instruksikan AI untuk output keputusan routing
2. Di Opal canvas, tambahkan kondisi routing pada panah keluar dari node
3. Opal evaluasi output Generate dan arahkan ke jalur yang tepat

**Cara mendefinisikan kondisi routing di canvas:**
```
Kondisi ditulis dalam bahasa natural di panel "Route Condition":
"Jika output mengandung KATEGORI: gambar"  → arahkan ke Node_Gambar
"Jika output mengandung KATEGORI: audio"   → arahkan ke Node_Audio
"Jika tidak ada kondisi yang cocok"        → arahkan ke Node_Fallback
```

**Format output Generate yang direkomendasikan untuk routing:**
```
KEPUTUSAN: [nama_jalur]
ALASAN: [satu kalimat]
```

Contoh output classifier yang bisa dideteksi router:
```
KEPUTUSAN: gambar
ALASAN: User menyebut "buat gambar dark fantasy"
```

---

#### 📁 NODE 5: Assets (Knowledge Base)

Tempat upload file referensi yang jadi "memori statis" agent.

**Format file yang didukung:**
- `.md` (Markdown) — **direkomendasikan** untuk rules/knowledge
- `.pdf` — dokumen panjang
- `.txt` — teks biasa
- `.docx` — dokumen Word
- File Google Drive (link langsung)

**Cara memanggil dari Generate:**
```
Baca panduan ini sebelum menjawab: @midjourney_rules
Gunakan knowledge dari: @suno_knowledge
```

**Tips Assets:**
- Beri nama file yang deskriptif — ini nama yang dipakai di `@`
- Split knowledge per topik — jangan satu file untuk semua
- File `.md` paling optimal karena terstruktur dan ringan

---

#### 📊 NODE 6: Output

Node terminal yang menampilkan hasil ke user.

**Format output yang tersedia:**
| Format | Kegunaan |
|--------|---------|
| **Text** | Tampilkan teks biasa atau markdown |
| **Webpage** | Render HTML/CSS interaktif (untuk kuis, dashboard, dll) |
| **File Download** | Output sebagai file yang bisa diunduh |
| **Image** | Tampilkan gambar yang digenerate |
| **Video** | Tampilkan video yang digenerate |

**Koneksi:** Output harus dihubungkan dengan panah dari Generate node yang menghasilkan data final.

---

### 1.3 Sistem Tag `@` — Koneksi Universal

`@` adalah cara semua node berkomunikasi satu sama lain. Ada tiga kategori:

```
@ → membuka dropdown dengan tiga pilar:

A. ASSETS    → @nama_file_asset
              Panggil file statis dari perpustakaan Assets
              Contoh: @midjourney_rules, @suno_knowledge

B. STEPS     → @nama_step
              Ambil output dari node lain di canvas
              Contoh: @UserInput_1, @Generate_Classifier, @Generate_Gambar

C. TOOLS     → @tools.nama_tool
              Aktifkan tool/kemampuan eksekusi
              Contoh: @tools.searchWeb, @tools.memory, @tools.goto
```

**Chaining dengan `@`:**
```
Generate node 3 bisa referensikan:
- @UserInput_1           (input awal user)
- @Generate_Classifier   (hasil klasifikasi dari step 2)
- @knowledge_midjourney  (asset knowledge model)
- @tools.memory          (preferensi user dari sesi sebelumnya)
```

---

### 1.4 Interactive Chat

Fitur di mana agent **menahan eksekusi** dan bertanya ke user sebelum lanjut.

**Cara mengaktifkan:** Di Generate node, pilih **Agent Mode** → Interactive Chat otomatis tersedia.

**Cara trigger dari System Prompt:**
```
Jika ada parameter yang kurang jelas atau belum disebutkan user,
tanyakan SATU pertanyaan klarifikasi sebelum melanjutkan.
Tunggu jawaban sebelum generate output final.
```

**Kapan digunakan untuk AGENT project:**
- Saat input user ambigu dan butuh klarifikasi
- Saat sub-agent mengumpulkan parameter (fixed steps)
- Saat preview prompt dan minta approval sebelum output final

---

### 1.5 Memory Tool — Detail Teknis

Memory Tool adalah fitur yang memungkinkan agent mengingat informasi **lintas sesi**.

**Cara kerja teknis:**
1. Informasi penting dari sesi ini disimpan di "memory bank" internal Opal
2. Saat sesi baru dimulai, memory bank dibaca dan diinjeksikan ke konteks agent
3. Agent "ingat" preferensi, riwayat, dan state dari sesi sebelumnya

**Cara mengaktifkan di System Prompt:**
```
Saat memulai sesi:
1. Baca memory dengan @tools.memory
2. Gunakan informasi dari memory untuk personalisasi respons
3. Sebutkan ke user jika ada preferensi yang diingat

Saat mengakhiri sesi:
1. Simpan preferensi penting ke memory dengan @tools.memory
2. Catat: model favorit, kategori terakhir, style preference
```

**Keterbatasan Memory Tool:**
- Memory internal Opal bersifat semi-persistent (bisa reset jika cache dihapus)
- Tidak ada kontrol granular atas apa yang disimpan
- Lebih cocok untuk preferensi ringan, bukan data kritikal

**Untuk data yang benar-benar persisten → gunakan Google Sheets (lihat Bagian 3)**

---

## BAGIAN 2 — CARA BUILD OPAL SECARA MANUAL (Step-by-Step)

### 2.1 Alur Build yang Direkomendasikan

```
Langkah 1: Buat struktur dasar dengan natural language
    → Deskripsikan tujuan workflow di prompt bar
    → Opal generate graph awal otomatis

Langkah 2: Buka Visual Editor
    → Switch ke mode "Build" / "Workflow"
    → Review semua node yang di-generate

Langkah 3: Konfigurasi setiap node manual
    → Klik setiap node → edit system prompt, model, tools
    → Rename node dengan nama deskriptif

Langkah 4: Atur koneksi dan routing
    → Hubungkan node dengan panah
    → Tambahkan kondisi routing di setiap jalur

Langkah 5: Upload Assets
    → Upload file .md untuk setiap cabang dan model knowledge

Langkah 6: Test dengan input sampel
    → Jalankan workflow dengan input test
    → Perhatikan animasi data flow — debug di node yang gagal

Langkah 7: Iterasi
    → Perbaiki system prompt yang bermasalah
    → Ulangi test sampai semua jalur berfungsi

Langkah 8: Publish
    → Generate shareable link
    → Share untuk feedback
```

---

### 2.2 Naming Convention di Canvas

Nama node yang konsisten sangat penting untuk chaining `@`.

| Node | Format Nama | Contoh |
|------|------------|--------|
| Input | `Input_[fungsi]` | `Input_UserGoal` |
| Generate Classifier | `Gen_Classifier` | `Gen_Classifier` |
| Generate per cabang | `Gen_[kategori]` | `Gen_Gambar`, `Gen_Audio` |
| Generate final | `Gen_Output` | `Gen_FinalPrompt` |
| Output | `Output_Final` | `Output_Final` |

---

### 2.3 Cara Build Workflow AGENT di Opal

Berikut cara mentranslasikan arsitektur AGENT ke dalam Opal canvas:

**Canvas yang dibutuhkan:**
```
[Input_UserGoal]
    ↓
[Gen_Classifier]  ← @tools.memory (baca preferensi)
    ↓
[Router]          ← Dynamic routing berdasarkan output classifier
    ↓          ↓          ↓          ↓          ↓          ↓
[Gen_Gambar] [Gen_Video] [Gen_Audio] [Gen_Code] [Gen_Persona] [Gen_Konten]
    ↓          ↓          (dll...)
[Gen_ModelSelector] ← @tools.memory + rekomendasikan model
    ↓
[Gen_InteractiveChat] ← Klarifikasi akhir, interactive chat aktif
    ↓
[Gen_FinalPrompt] ← @asset_knowledge_model
    ↓
[Gen_Preview]     ← Tampilkan draft, minta approval
    ↓
[Output_Final]    ← @tools.memory (simpan preferensi)
```

**System Prompt untuk `Gen_Classifier`:**
```
Kamu adalah AGENT — intelligent prompt engineering classifier.

Baca preferensi user dari: @tools.memory

Analisis input berikut: @Input_UserGoal

Tentukan kategori:
- gambar: konten visual statis
- video: konten bergerak
- audio: musik/suara
- coding: kode program
- persona: karakter AI
- konten: tulisan/narasi

Jika ambigu: tanya SATU pertanyaan klarifikasi.

Output format:
KEPUTUSAN: [kategori]
ALASAN: [satu kalimat]
CATATAN: [info tambahan untuk sub-agent]
```

**Cara set routing dari `Gen_Classifier`:**
```
Jalur 1: output mengandung "KEPUTUSAN: gambar" → Gen_Gambar
Jalur 2: output mengandung "KEPUTUSAN: video"  → Gen_Video
Jalur 3: output mengandung "KEPUTUSAN: audio"  → Gen_Audio
Jalur 4: output mengandung "KEPUTUSAN: coding" → Gen_Code
Jalur 5: output mengandung "KEPUTUSAN: persona" → Gen_Persona
Jalur 6: output mengandung "KEPUTUSAN: konten" → Gen_Konten
Fallback: semua kondisi gagal → Gen_Classifier (loop kembali)
```

---

## BAGIAN 3 — ADVANCED MEMORY MANAGEMENT

### 3.1 Arsitektur Memory Dua Lapis

Untuk project AGENT, gunakan **dua lapis memory** yang saling melengkapi:

```
LAPIS 1: Opal Native Memory (@tools.memory)
    → Untuk: preferensi ringan sesi-ke-sesi
    → Simpan: model favorit, kategori terakhir, nama user
    → Kelebihan: otomatis, zero-config
    → Kelemahan: semi-persistent, tidak ada kontrol granular

LAPIS 2: Google Sheets (External State Machine)
    → Untuk: data kritikal yang harus 100% persisten
    → Simpan: riwayat prompt, profil lengkap, feedback, history
    → Kelebihan: visible, auditable, bisa di-query
    → Kelemahan: perlu setup manual
```

### 3.2 Setup Google Sheets sebagai Persistent Memory

**Langkah 1: Buat spreadsheet dengan 4 tab**
```
Tab 1: users     → profil dan preferensi user
Tab 2: history   → riwayat semua prompt yang dibuat
Tab 3: model_kb  → knowledge model (planned)
Tab 4: feedback  → rating dan feedback
```

**Langkah 2: Beri akses ke Opal**
- Bagikan Google Sheets ke akun Google yang digunakan Opal
- Atau gunakan Google Drive integration yang tersedia di Opal

**Langkah 3: Instruksikan agent di System Prompt**

*Untuk node yang MEMBACA memory (di awal sesi — Gen_Classifier):*
```
Sebelum merespons, baca data user dari Google Sheets ini:
[link atau referensi @asset_sheets_config]

Cari baris dengan user_id yang cocok dengan sesi ini.
Jika ditemukan: gunakan favorite_model dan preferred_style untuk personalisasi.
Jika tidak ditemukan: buat profil baru untuk user ini.
```

*Untuk node yang MENULIS memory (di akhir sesi — Output_Final):*
```
Setelah prompt final di-approve, simpan ke Google Sheets:
- user_id: [dari sesi]
- timestamp: [sekarang]
- kategori: [KEPUTUSAN dari classifier]
- model: [model yang dipilih]
- prompt: [prompt yang digenerate]
- catatan preferensi: [style atau pilihan yang disebutkan user]
```

### 3.3 Hack: Sheets sebagai State Machine

Teknik ini memungkinkan workflow multi-sesi yang kompleks tanpa kehilangan konteks.

**Cara kerja:**
```
Sesi 1:
User → Agent kumpulkan parameter → Simpan progress ke Sheets → Sesi berakhir

Sesi 2:
Agent baca Sheets → Lihat progress sesi 1 → Lanjutkan dari titik terakhir

Sesi 3:
Agent baca Sheets → Semua parameter lengkap → Generate final prompt
```

**Schema untuk state machine di Sheets:**
```
Kolom:
A: session_id    → ID sesi
B: user_id       → ID user
C: status        → "in_progress" / "completed"
D: step_reached  → step terakhir yang diselesaikan (misal: "3_of_5")
E: param_subject → parameter yang sudah terkumpul
F: param_style   → parameter yang sudah terkumpul
G: param_model   → parameter yang sudah terkumpul
H: timestamp     → kapan terakhir update
```

**System prompt untuk resume sesi:**
```
Cek Sheets untuk sesi dengan user_id ini:
- Jika status = "in_progress": lanjutkan dari step_reached
- Jika status = "completed": tanya user mau buat prompt baru atau edit yang lama
- Jika tidak ada: mulai sesi baru

Jangan tanya ulang parameter yang sudah tersimpan di kolom param_*.
```

---

## BAGIAN 4 — ADVANCED HACKS & PATTERNS

### 4.1 Hack: Assets sebagai Modular Knowledge Injection

Daripada menulis semua knowledge di system prompt (yang membuat prompt jadi panjang dan susah di-maintain), gunakan Assets.

**Cara:**
```
Di Assets:
- Upload: midjourney_rules.md
- Upload: suno_rules.md
- Upload: dalle3_rules.md
- Upload: classifier_rules.md

Di System Prompt Gen_FinalPrompt:
"Berdasarkan model yang dipilih (@Gen_ModelSelector),
muat knowledge yang sesuai:
- Jika Midjourney: @midjourney_rules
- Jika Suno: @suno_rules
- Jika DALL-E: @dalle3_rules"
```

**Keuntungan:**
- Update knowledge model = cukup replace file Asset, tidak perlu buka canvas
- System prompt tetap bersih dan pendek
- Bisa A/B test knowledge yang berbeda tanpa mengubah workflow

### 4.2 Hack: Output sebagai JSON untuk Routing yang Lebih Presisi

Daripada mendeteksi kata "KEPUTUSAN: gambar" dari text biasa (rentan false positive), instruksikan Generate node untuk output JSON.

**System Prompt:**
```
Output HANYA dalam format JSON ini, tidak ada teks lain:
{
  "keputusan": "gambar",
  "konfidenssi": "tinggi",
  "alasan": "...",
  "catatan": "..."
}
```

**Kondisi routing:**
```
Jalur 1: output mengandung '"keputusan": "gambar"' → Gen_Gambar
Jalur 2: output mengandung '"keputusan": "video"'  → Gen_Video
(dst.)
```

### 4.3 Hack: Loop Pattern untuk Iterasi Parameter

Untuk sub-agent yang mengumpulkan parameter satu per satu, gunakan loop dengan state tracking.

**Cara kerja:**
```
Gen_SubAgent_Gambar
├── Baca state dari @tools.memory
├── Jika step = 1: tanya subjek → simpan → goto step 2
├── Jika step = 2: tanya style  → simpan → goto step 3
├── Jika step = 3: tanya mood   → simpan → goto step 4
├── Jika step = 4: tanya ratio  → simpan → goto step 5
└── Jika step = 5: semua terkumpul → @tools.goto → Gen_ModelSelector
```

**System prompt dengan state tracking:**
```
Baca state saat ini dari @tools.memory (key: "param_collection_step").

Jika step = null atau 1:
  Tanya: "Apa subjek utama gambarnya?"
  Setelah dijawab: simpan ke memory, set step = 2

Jika step = 2:
  Tanya: "Style visual apa yang diinginkan?"
  [dst.]

Jika semua step selesai:
  Output: PARAMETER_COMPLETE: true
  Arahkan ke model selector.
```

### 4.4 Hack: "Wakeup Call" untuk Context Recovery

Untuk workflow panjang di mana agent bisa kehilangan konteks, tambahkan "Wakeup Call" step.

**Cara:**
```
Tambahkan Generate node khusus setelah 3-4 step:

System Prompt "Gen_ContextCheck":
"Sebelum melanjutkan, verifikasi state saat ini:
1. Apa yang sudah dilakukan? (baca @Gen_step1, @Gen_step2, dst.)
2. Apa yang masih perlu dilakukan?
3. Apa keputusan penting yang sudah dibuat?

Output ringkasan dalam 3-5 bullet points, lalu lanjutkan."
```

**Kapan digunakan:** Sebelum Generate final jika ada 5+ step sebelumnya.

### 4.5 Hack: Parallel Branch Merge

Untuk menggabungkan output dari beberapa cabang sebelum Generate final.

**Cara:**
```
[Input] → [Classifier] → [Gen_Gambar] → [Gen_Merger]
                      → [Gen_Video]  ↗
                      → [Gen_Audio]  ↗

System Prompt Gen_Merger:
"Gabungkan semua informasi dari:
- @Gen_Gambar (jika ada)
- @Gen_Video  (jika ada)
- @Gen_Audio  (jika ada)

Buat satu brief komprehensif untuk generate final."
```

### 4.6 Hack: Structured Preview dengan Approval Gate

Sebelum output final, buat preview step yang meminta konfirmasi user.

**System Prompt Gen_Preview:**
```
Berdasarkan semua parameter yang terkumpul:
@Gen_SubAgent, @Gen_ModelSelector

Buat DRAFT prompt dan tampilkan dengan format ini:

═══ DRAFT PROMPT KAMU ═══
[prompt yang digenerate]

📌 Model target: [model]
📁 Kategori: [kategori]
━━━━━━━━━━━━━━━━━━━━━━━

Ketik:
✅ "oke" atau "approve" → prompt final
✏️ "[hal yang mau diubah]" → revisi spesifik
🔄 "ulang" → kembali ke awal

Tunggu respons sebelum melanjutkan.
```

**Routing dari Gen_Preview:**
```
Jika output mengandung "approve" atau "oke" → Output_Final
Jika output mengandung "revisi" atau "ubah" → Gen_SubAgent (loop)
Jika output mengandung "ulang" → Gen_Classifier (mulai ulang)
```

---

## BAGIAN 5 — KETERBATASAN OPAL & WORKAROUND

### 5.1 Keterbatasan yang Perlu Diketahui

| Keterbatasan | Dampak | Workaround |
|-------------|--------|-----------|
| **Tidak ada webhook/trigger** | Tidak bisa auto-run saat ada event eksternal | Jadikan manual trigger + Google Apps Script sebagai middleware |
| **Tidak ada cron/scheduling** | Tidak bisa jadwalkan workflow otomatis | Buat tombol manual "Run Daily Report" |
| **Memory internal semi-persistent** | Data bisa hilang jika cache dibersihkan | Gunakan Google Sheets sebagai backup memory |
| **Context window terbatas** | Workflow panjang bisa kehilangan konteks awal | Tambahkan "Wakeup Call" step + context compression |
| **Tidak ada public API** | Tidak bisa integrasikan ke aplikasi external | Gunakan shareable link, atau migrasi ke Vertex AI Agent Builder |
| **Single user session** | Tidak ada multi-user management bawaan | Implementasikan user_id di Google Sheets |
| **Debugging terbatas** | Error tidak selalu jelas | Jalankan node satu per satu di test mode |

### 5.2 Kapan Harus Migrasi dari Opal

Opal **tidak lagi cukup** ketika:
- Butuh background automation tanpa trigger manual
- Butuh webhook dari sistem external
- Butuh multi-user dengan role management
- Production traffic yang tinggi dan SLA yang ketat
- Butuh audit trail yang lengkap

**Alternatif yang direkomendasikan:**
- **Google Vertex AI Agent Builder** — untuk production-grade agent
- **Google ADK (Agent Development Kit)** — untuk developer yang mau koding
- **n8n atau Zapier** — jika fokus pada workflow automation lintas aplikasi

---

## BAGIAN 6 — CHECKLIST BUILD AGENT PROJECT DI OPAL

### Pre-Build
```
□ Semua file .md knowledge sudah disiapkan di 02_branches/ dan 03_models/
□ Google Sheets sudah dibuat dengan 4 tab (users, history, model_kb, feedback)
□ Naming convention sudah ditentukan
□ System prompt untuk setiap node sudah ditulis di 01_core/
```

### Build Phase
```
□ Canvas dibuat dengan node yang benar
□ Semua node sudah diberi nama sesuai konvensi
□ Assets sudah diupload (semua file .md dari knowledge base)
□ System prompt Gen_Classifier sudah dikonfigurasi
□ Dynamic routing sudah ditambahkan (6 jalur + fallback)
□ Sub-agent per cabang sudah dikonfigurasi
□ Memory tool diaktifkan di Gen_Classifier dan Output_Final
□ Google Sheets connection dikonfigurasi
```

### Test Phase
```
□ Test input gambar yang jelas → harus ke Gen_Gambar
□ Test input video yang jelas  → harus ke Gen_Video
□ Test input ambigu            → harus trigger klarifikasi
□ Test sesi kedua (memory)     → harus ingat preferensi
□ Test approval flow           → harus bisa revisi
□ Test fallback                → tidak boleh error
```

---

## Changelog Dokumen

| Versi | Tanggal | Perubahan |
|-------|---------|-----------|
| 1.0 | Mei 2026 | Versi awal — anatomi dasar 6 node |
| 2.0 | Mei 2026 | Rewrite lengkap — tambah Agent Mode, memory hacks, advanced patterns, keterbatasan & workaround |
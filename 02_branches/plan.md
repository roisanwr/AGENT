# Plan — Layer 02_branches & 03_models

> Status: **DRAFT v1.0** | Dibuat: Mei 2026
> Dokumen ini adalah roadmap lengkap semua yang perlu disiapkan, diputuskan, dan dibangun untuk layer cabang dan knowledge model AGENT.

---

## 📊 Status Inventaris Saat Ini

### Yang Sudah Ada ✅

**02_branches (Sub-agent spec — konten & parameter):**
| File | Status |
|------|--------|
| `image.md` | ✅ Ada — perlu audit & update |
| `video.md` | ✅ Ada — perlu audit & update |
| `audio.md` | ✅ Ada — perlu audit & update |
| `coding.md` | ✅ Ada — perlu audit & update |
| `persona.md` | ✅ Ada — perlu audit & update |
| `content.md` | ✅ Ada — perlu audit & update |
| `_template.md` | ✅ Ada |

**03_models (Knowledge per model AI):**
| Model | File | Status |
|-------|------|--------|
| Midjourney v6+ | `image/midjourney.md` | ✅ Ada |
| DALL-E 3 | `image/dalle3.md` | ✅ Ada |
| Suno v4 | `audio/suno.md` | ✅ Ada |
| Google Veo | `video/veo.md` | ✅ Ada |
| Claude | `text/claude.md` | ✅ Ada |
| GPT-4o/5 | `text/gpt.md` | ✅ Ada |

### Yang Belum Ada — Harus Dibuat ❌

**03_models — Missing Knowledge Files:**
| Model | File Target | Prioritas |
|-------|------------|----------|
| Stable Diffusion (SDXL/SD3) | `image/stable_diffusion.md` | 🔴 Tinggi |
| Flux | `image/flux.md` | 🔴 Tinggi |
| Ideogram v2 | `image/ideogram.md` | 🟡 Sedang |
| **Nano Banana (Opal internal)** | `image/nano_banana.md` | 🔴 Tinggi — model Opal sendiri |
| Sora | `video/sora.md` | 🟡 Sedang |
| Kling | `video/kling.md` | 🟡 Sedang |
| Runway Gen-3 Alpha | `video/runway.md` | 🟡 Sedang |
| **Veo (Opal internal)** | `video/veo_opal.md` | 🔴 Tinggi — model Opal sendiri |
| Udio | `audio/udio.md` | 🟡 Sedang |
| ElevenLabs | `audio/elevenlabs.md` | 🟡 Sedang |
| **Lyria 2 (Opal internal)** | `audio/lyria2.md` | 🔴 Tinggi — model Opal sendiri |
| **AudioLM (Opal internal)** | `audio/audiolm.md` | 🟡 Sedang |
| Gemini 3 | `text/gemini.md` | 🔴 Tinggi |

**02_branches — Perlu Diaudit & Diperbaiki:**
Semua 6 file cabang yang ada perlu diaudit karena:
1. System prompt sub-agent di dalamnya belum mengintegrasikan **persona Yui** (`@agent_persona`)
2. Format output handoff belum selaras dengan format baru (`SIAP_GENERATE: true`)
3. Belum ada instruksi **Opal-specific** (cara panggil tools, cara referensikan step sebelumnya)

---

## 🏗️ Arsitektur Node per Cabang di Opal Canvas

Sebelum mengisi file, penting untuk memahami pola node yang akan dibangun di Opal untuk SETIAP cabang. Semua cabang mengikuti pola yang sama:

```
[Gen_SubAgent_[Kategori]]   ← menerima dari Gen_Classifier
        ↓                      @agent_persona (Yui tetap aktif)
   (parameter collection)      @Gen_Classifier (baca data Discovery)
        ↓                      @tools.memory (cek preferensi lama)
        ↓ (SIAP_GENERATE: true)
[Gen_ModelSelector_[Kategori]]
        ↓                      Rekomendasi 1-2 model + konfirmasi user
        ↓ (MODEL_DIPILIH: [model])
[Gen_FinalPrompt_[Kategori]]
        ↓                      @agent_persona
        ↓                      @[knowledge_model_yang_dipilih]
        ↓                      @quality_gates
        ↓ (prompt teks selesai)
[Gen_Preview]                ← SHARED — satu node untuk semua cabang
        ↓                      Tampilkan draft, minta approve/revisi
        ↓ (APPROVED: true)
[Output_Final]               ← SHARED — satu node output
                               @tools.memory (simpan preferensi)
```

> **Catatan:** `Gen_Preview` dan `Output_Final` adalah node **shared** yang menerima input dari semua cabang. Ini menghemat node dan memudahkan maintenance.

---

## 📋 Rencana Kerja: Apa yang Perlu Dibuat & Urutannya

### FASE 1 — Audit & Standardisasi (Lakukan Pertama)

**Tujuan:** Selaraskan semua file yang sudah ada dengan standar baru (persona Yui, format handoff, referensi Opal).

Setiap file `02_branches/[cabang].md` perlu diaudit dan dipastikan memiliki:

- [ ] `@agent_persona` disebut di system prompt
- [ ] Format handoff output: `SIAP_GENERATE: true` + daftar parameter
- [ ] Referensi ke step Opal (`@Gen_Classifier`, `@tools.memory`)
- [ ] Minimal 4 parameter dengan default yang jelas
- [ ] Template prompt output per model yang dituju
- [ ] Quality checklist yang spesifik

**File yang perlu diaudit:**
- [ ] `02_branches/image.md`
- [ ] `02_branches/video.md`
- [ ] `02_branches/audio.md`
- [ ] `02_branches/coding.md`
- [ ] `02_branches/persona.md`
- [ ] `02_branches/content.md`

---

### FASE 2 — Buat Knowledge Files yang Hilang (Kritis)

Ini adalah **isi utama** yang digunakan oleh `Gen_FinalPrompt` untuk tahu **sintaks dan cara nulis** prompt yang benar untuk setiap model.

**Struktur yang WAJIB ada di setiap knowledge file:**

```markdown
# Knowledge: [Nama Model]

## Apa itu [Model]?
## Cara Kerja Prompt di [Model]
## Sintaks & Parameter Wajib
## Parameter Opsional & Efeknya
## Template Prompt (Copy-Paste Ready)
## 5 Contoh Prompt Output Berkualitas Tinggi
## Yang TIDAK Boleh Dilakukan (Common Mistakes)
## Perbedaan dari Model Lain
```

**Urutan pembuatan (prioritas):**

**🔴 Fase 2A — Prioritas Kritis (Model Opal Internal):**

| # | File | Alasan Prioritas |
|---|------|----------------|
| 1 | `image/nano_banana.md` | Model gambar INTERNAL Opal — user bisa pakai langsung |
| 2 | `audio/lyria2.md` | Model musik INTERNAL Opal — user bisa pakai langsung |
| 3 | `video/veo_opal.md` | Sudah ada `veo.md` tapi perlu versi khusus Opal |
| 4 | `text/gemini.md` | Engine utama sistem kita adalah Gemini |

**🟡 Fase 2B — Prioritas Tinggi (Model Eksternal Populer):**

| # | File | Alasan |
|---|------|--------|
| 5 | `image/stable_diffusion.md` | Salah satu model gambar paling banyak digunakan |
| 6 | `image/flux.md` | Sedang naik daun, alternatif SD terbaik 2025-2026 |
| 7 | `audio/udio.md` | Alternatif Suno, banyak digunakan |
| 8 | `audio/elevenlabs.md` | Satu-satunya untuk voice/TTS |
| 9 | `video/sora.md` | Model video paling terkenal setelah Veo |

**🟢 Fase 2C — Pelengkap:**

| # | File | Alasan |
|---|------|--------|
| 10 | `image/ideogram.md` | Spesialisasi teks dalam gambar |
| 11 | `video/kling.md` | Alternatif video dari referensi gambar |
| 12 | `video/runway.md` | Efek visual kreatif |
| 13 | `audio/audiolm.md` | Speech Opal internal |

---

### FASE 3 — Buat Node di Opal Canvas per Cabang

Setelah semua file siap, node-node berikut perlu dibuat di canvas Opal untuk setiap cabang:

**Per cabang (×6 = 18 node baru):**

| Node | Nama di Canvas | Mode | Model |
|------|--------------|------|-------|
| Sub-agent parameter | `Gen_SubAgent_[Kategori]` | Agent + Interactive Chat | Gemini 3 Flash |
| Model selector | `Gen_ModelSelector_[Kategori]` | Agent + Interactive Chat | Gemini 3 Flash |
| Prompt generator | `Gen_FinalPrompt_[Kategori]` | Fixed | Gemini 3.1 Pro |

**Shared node (×2 = dibuat sekali):**

| Node | Nama di Canvas | Mode |
|------|--------------|------|
| Preview & approval | `Gen_Preview` | Agent + Interactive Chat |
| Output final | `Output_Final` | Output (Text) |

---

### FASE 4 — Setup Shared Node: Preview & Output

**`Gen_Preview` — System Prompt:**
```
@agent_persona

Kamu menerima sebuah draft prompt dari sub-agent.

Tampilkan dengan format ini:

═══ DRAFT PROMPT KAMU ═══
[isi prompt yang diterima dari @Gen_FinalPrompt_*]

📌 Model target: [model yang dipilih]
📁 Kategori: [kategori]
━━━━━━━━━━━━━━━━━━━━━━━

Ketik:
✅ "oke" atau "approve" → langsung ke output final
✏️ "[hal yang mau diubah]" → Yui akan revisi spesifik bagian itu
🔄 "ulang" → mulai ulang dari awal

Tunggu respons user.
```

**Kondisi routing dari `Gen_Preview`:**
- `"oke"` atau `"approve"` → `Output_Final`
- `"ubah"` atau `"revisi"` → `Gen_FinalPrompt_[Kategori]` (regenerate)
- `"ulang"` → `Input_UserGoal` (mulai dari awal)

**`Output_Final` — System Prompt:**
```
@agent_persona

Tampilkan prompt final dalam format ini:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ PROMPT SIAP DIPAKAI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[prompt final]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 Tips: Kamu bisa langsung copy prompt ini dan pakai di [model target].

Mau bikin prompt lain? Ketik aja, aku siap! 🚀

Simpan preferensi ke @tools.memory:
- model favorit: [model]
- kategori terakhir: [kategori]
- style preference: [jika disebutkan]
```

---

### FASE 5 — README per Cabang

Sama seperti `01_core/README.md`, setiap cabang perlu punya **README.md** yang berisi:
- Penjelasan cabang ini untuk apa
- Daftar node yang perlu dibuat di Opal
- Step-by-step cara setup node tersebut
- Cara menghubungkan ke Gen_Preview (shared)
- Test cases untuk validasi

**File yang perlu dibuat:**
- [ ] `02_branches/README.md` (overview semua cabang)
- [ ] `02_branches/image_readme.md` (instalasi cabang gambar)
- [ ] `02_branches/video_readme.md`
- [ ] `02_branches/audio_readme.md`
- [ ] `02_branches/coding_readme.md`
- [ ] `02_branches/persona_readme.md`
- [ ] `02_branches/content_readme.md`

---

## ❓ Pertanyaan yang Perlu Kamu Jawab

Semua keputusan di bawah ini berdampak langsung pada cara kita membangun sistem. Jawab sekaligus, aku akan implementasikan semuanya setelah dapat jawaban.

---

### Q1 — Scope Model v1.0 (Kritis)

Dari daftar model ini, **mana yang WAJIB ada di peluncuran pertama** dan mana yang bisa ditambahkan nanti?

**Kategori Gambar:**
- [ ] Midjourney v6+ *(sudah ada knowledge filenya)*
- [ ] DALL-E 3 *(sudah ada knowledge filenya)*
- [ ] Nano Banana Opal Internal *(belum ada, kritis karena Opal native)*
- [ ] Stable Diffusion *(belum ada)*
- [ ] Flux *(belum ada)*
- [ ] Ideogram v2 *(belum ada)*

**Kategori Video:**
- [ ] Veo (Opal/Google) *(sudah ada knowledge filenya)*
- [ ] Sora *(belum ada)*
- [ ] Kling *(belum ada)*
- [ ] Runway *(belum ada)*

**Kategori Audio:**
- [ ] Suno v4 *(sudah ada knowledge filenya)*
- [ ] Lyria 2 Opal Internal *(belum ada, kritis karena Opal native)*
- [ ] Udio *(belum ada)*
- [ ] ElevenLabs *(belum ada)*

**Kategori Teks/Coding/Persona:**
- [ ] Claude *(sudah ada knowledge filenya)*
- [ ] GPT-4o/5 *(sudah ada knowledge filenya)*
- [ ] Gemini 3 *(belum ada!)*

---

### Q2 — Mekanisme Pemilihan Model

Saat user sudah selesai di sub-agent dan masuk ke Model Selector, bagaimana cara pemilihan model:

**Opsi A:** Yui (AI) merekomendasikan 1 model terbaik berdasarkan parameter yang dikumpulkan → user konfirmasi yes/no → langsung proceed.

**Opsi B:** Yui menawarkan list pilihan model (1-4 opsi) dengan kelebihan masing-masing → user pilih sendiri.

**Opsi C:** Hybrid — jika user sudah menyebut model di awal (Discovery), langsung pakai itu. Jika tidak, tawarkan rekomendasi.

> *Mana yang kamu pilih? Atau ada kombinasi lain?*

---

### Q3 — Bahasa Output Prompt

Prompt akhir yang dihasilkan sistem untuk diberikan ke user — dalam bahasa apa?

**Opsi A:** Selalu **Bahasa Inggris** — karena semua model AI (Midjourney, Suno, dll.) menghasilkan output terbaik dengan prompt Inggris.

**Opsi B:** Ikut **bahasa user** — jika user pakai Indonesia, prompt output dalam Indonesia.

**Opsi C:** **Dual-output** — tampilkan prompt Inggris (untuk di-copy ke model AI) + terjemahan Indonesia (untuk user mengerti apa yang di-prompt-kan).

> *Rekomendasi aku: Opsi A atau C. Opsi B berisiko menghasilkan prompt yang kurang optimal untuk model gambar/video/audio.*

---

### Q4 — Fitur Variation Mode

Apakah kamu mau fitur di mana setelah parameter terkumpul, sistem menghasilkan **2-3 variasi prompt** (berbeda tone/style) untuk user pilih, alih-alih langsung 1 prompt?

**Opsi A:** Ya — generate 3 variasi → user pilih yang paling cocok → output variasi yang dipilih.

**Opsi B:** Tidak — generate 1 prompt terbaik → user bisa minta revisi jika tidak cocok.

**Opsi C:** Tanyakan ke user di setiap sesi ("Mau satu prompt atau mau aku kasih 3 variasi?")

> *Opsi C paling fleksibel tapi menambah 1 pertanyaan lagi ke alur. Opsi A memberikan nilai lebih tapi 3x output = lebih lambat.*

---

### Q5 — Multi-Kategori dalam Satu Sesi

Kalau user minta sesuatu yang melibatkan dua kategori (contoh: "Buatin gambar + musik yang cocok untuk gambar itu"), bagaimana handling-nya?

**Opsi A:** Tangani satu per satu dalam sesi yang sama — selesaikan gambar dulu, lalu otomatis lanjut ke musik.

**Opsi B:** Tolak multi-kategori — informasikan ke user bahwa setiap sesi hanya untuk satu kategori.

**Opsi C:** Tangani tapi pisah — generate prompt gambar, tampilkan, lalu tanya "Mau lanjut buat prompt musiknya?"

---

### Q6 — Kedalaman Parameter Collection

Untuk setiap cabang, seberapa dalam kita menggali parameter?

**Opsi A (Minimalist — 3-4 parameter wajib):** Cepat, tidak melelahkan. Cocok untuk user yang sudah cukup tau.

**Opsi B (Standard — 5-6 parameter, wajib + opsional):** Seimbang antara kecepatan dan kualitas.

**Opsi C (Exhaustive — 7+ parameter):** Prompt yang sangat detail dan presisi, tapi prosesnya lebih panjang.

> *Rekomendasi aku: Opsi B dengan pendekatan "progressive disclosure" — 3-4 parameter wajib ditanya, 2-3 parameter opsional hanya ditanya jika user bilang "mau lebih detail".*

---

### Q7 — Cabang Baru yang Ingin Ditambah?

Selain 6 cabang yang sudah ada (Gambar, Video, Audio, Coding, Persona, Konten), apakah ada kategori lain yang ingin kamu tambahkan?

Beberapa kandidat populer:
- [ ] **Presentasi/Slide** — prompt untuk AI pembuat slide (Gamma, Tome, dll.)
- [ ] **3D Modeling** — prompt untuk Meshy, Tripo3D
- [ ] **UI/UX Design** — prompt untuk V0, Lovable, Bolt
- [ ] **Iklan/Ad Copy** — prompt untuk Facebook Ads, Google Ads
- [ ] **Foto Editing** — prompt untuk Adobe Firefly, Lightroom AI

---

### Q8 — Integrasi Google Sheets (Memory Layer)

Apakah kamu sudah ingin setup Google Sheets sebagai persistent memory sekarang, atau ini bisa ditunda ke fase berikutnya?

Google Sheets dibutuhkan untuk menyimpan:
- Riwayat prompt yang dibuat user
- Model favorit user
- Style preference lintas sesi

**Jika ya:** Kita perlu buat skema spreadsheet dulu (sudah ada di `04_memory/schema.md`).
**Jika tidak sekarang:** System akan pakai `@tools.memory` Opal yang semi-persistent.

---

### Q9 — Proof of Concept (PoC) Prioritas

Sebelum membangun semua 6 cabang sekaligus, lebih baik kita build satu cabang sampai selesai end-to-end sebagai PoC untuk validasi.

**Cabang mana yang kamu mau prioritaskan untuk PoC?**

- [ ] **Gambar** — paling banyak use case, knowledge sudah paling lengkap
- [ ] **Audio** — Suno sangat populer, knowledge sudah ada
- [ ] **Coding** — mungkin paling sering dipakai secara personal
- [ ] *(pilihan lain)*

> *Rekomendasi aku: mulai dengan **Gambar** karena knowledge file sudah ada 2 model (Midjourney + DALL-E 3) dan use case-nya paling mudah untuk divalidasi secara visual.*

---

## 🗺️ Ringkasan Total Yang Perlu Dibuat

| Kategori | File | Jumlah |
|----------|------|--------|
| Knowledge file model baru (03_models) | Semua file yang belum ada | **13 file** |
| Audit & update branch files (02_branches) | 6 file yang sudah ada | **6 file** |
| README per cabang | Panduan instalasi Opal per cabang | **7 file** |
| Shared node spec | Gen_Preview + Output_Final | **2 spec** |
| Total | | **~28 deliverable** |

---

## 📅 Timeline Usulan (Berdasarkan Diskusi)

```
MINGGU 1 — PoC (Satu Cabang End-to-End)
  ├── Q1-Q9 dijawab
  ├── Audit image.md
  ├── Buat nano_banana.md, stable_diffusion.md, flux.md
  ├── Build canvas Opal: cabang Gambar (4 node)
  └── Test PoC dengan 10 input berbeda

MINGGU 2 — Ekspansi Audio & Video
  ├── Audit audio.md dan video.md
  ├── Buat lyria2.md, udio.md, elevenlabs.md
  ├── Buat sora.md, kling.md, runway.md
  └── Build canvas Opal: cabang Audio + Video

MINGGU 3 — Cabang Teks (Coding, Persona, Konten)
  ├── Audit coding.md, persona.md, content.md
  ├── Buat gemini.md
  └── Build canvas Opal: 3 cabang teks

MINGGU 4 — Polish & Integrasi
  ├── Setup Google Sheets (jika Q8 = ya)
  ├── Build Gen_Preview dan Output_Final (shared)
  ├── Full integration test semua cabang
  └── Publish link pertama untuk user feedback
```

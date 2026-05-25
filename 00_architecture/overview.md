# Gambaran Arsitektur — AGENT Opal Prompt Builder

## Apa Itu Sistem Ini?

AGENT adalah sebuah **intelligent prompt engineering system** yang dibangun di atas platform Google Opal. Tujuannya satu: menghasilkan prompt AI yang benar-benar bagus — bukan prompt generik, tapi prompt yang sudah disesuaikan dengan **kategori konten** sekaligus **model AI target** yang akan mengeksekusinya.

### Masalah yang Dipecahkan

> Prompt satu jalur = hasil biasa-biasa saja.

Prompt untuk Midjourney sangat berbeda dengan prompt untuk Suno. Prompt coding untuk Claude berbeda strukturnya dengan untuk GPT-4o. Sistem ini menyelesaikan masalah ini dengan pendekatan **branching + model-awareness**: setiap jenis konten ditangani oleh sub-agent khusus yang memahami "bahasa" model AI yang dituju.

---

## Lapisan Arsitektur

Sistem ini terdiri dari **5 lapisan utama** yang saling terhubung:

```
┌─────────────────────────────────────────────────────────┐
│                    LAPISAN 1                            │
│              INPUT & CONTEXT GATHERING                  │
│         (User menyampaikan tujuan & konteks)           │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                    LAPISAN 2                            │
│          AGENT CLASSIFIER + ROUTER (Opal)               │
│   (Gemini menganalisis, mengklasifikasi, merouting)     │
│   ↔ Memory: baca preferensi user dari sesi sebelumnya   │
└────────────┬────────────────────────────────────────────┘
             │
    ┌────────▼─────────────────────────────────────┐
    │              LAPISAN 3                       │
    │         CABANG SPESIFIK                      │
    │  [Gambar][Video][Audio][Coding][Persona]...  │
    └────────┬─────────────────────────────────────┘
             │
┌────────────▼────────────────────────────────────────────┐
│                    LAPISAN 4                            │
│         MODEL SELECTOR + KNOWLEDGE LAYER                │
│   (Pilih model AI, load knowledge spesifik model)       │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                    LAPISAN 5                            │
│          GENERATE → PREVIEW → OUTPUT                    │
│    (Susun prompt, iterasi, simpan ke memory)            │
└─────────────────────────────────────────────────────────┘
```

---

## Komponen Utama

### 1. Entry Point — Input Step
Titik masuk tunggal untuk semua permintaan. User tidak perlu pilih kategori secara manual — mereka cukup mengetik tujuan dalam bahasa natural.

**Yang dikumpulkan:**
- Tujuan / intention user (wajib)
- Konteks tambahan (opsional)
- Target model jika sudah tahu (opsional)

### 2. Agent Classifier (Opal Agent Step)
Ini adalah "otak" routing sistem. Powered by Gemini Flash, bertugas sebagai **Silent Router**:

- **Menganalisis** payload dari Discovery Stage
- **Mengklasifikasi** ke salah satu kategori yang tersedia secara *silent* (tanpa chat ke user)
- **Membaca memory** preferensi user dari sesi sebelumnya
- **Merutekan** ke cabang yang tepat via dynamic routing

### 3. Dynamic Router
Mengeksekusi keputusan routing dari classifier. Memiliki kondisi untuk setiap cabang dan satu fallback "lainnya" untuk kategori baru yang belum ada.

> **Aturan emas routing:** Classifier adalah Silent Router. Jika ragu atau input tidak memenuhi satupun kondisi cabang, lempar ke cabang `fallback` untuk diurus kembali oleh sistem klarifikasi, jangan berasumsi sendiri.

### 4. Sub-Agent per Cabang (Fixed Steps)
Setiap cabang adalah unit yang independen dengan fixed steps sendiri. Mereka bertanggung jawab untuk:
- Menggali parameter spesifik yang dibutuhkan cabang tersebut
- Bertanya satu pertanyaan pada satu waktu (tidak overwhelming)
- Memvalidasi kelengkapan parameter sebelum lanjut ke generate

### 5. Model Selector
Layer baru yang menambahkan dimensi kedua: selain kategori konten, sistem juga memilih **target model AI** yang tepat.

- Agent merekomendasikan model berdasarkan konteks
- User mengkonfirmasi atau memilih sendiri
- Knowledge spesifik model di-load dari `03_models/`

### 6. Generate Step
Menyusun prompt final menggunakan semua parameter yang sudah dikumpulkan + knowledge model spesifik.

### 7. Preview & Iterasi
User bisa review draft prompt sebelum menerima output final. Ada feedback loop untuk revisi.

### 8. Output + Memory
- Prompt final siap di-copy atau dishare
- Preferensi user disimpan ke Google Sheets untuk sesi berikutnya

---

## Filosofi Desain

### "Modular by Default"
Setiap cabang adalah unit yang sepenuhnya independen. Menambah cabang baru = hanya mendefinisikan path baru + fixed steps-nya, tanpa menyentuh cabang yang sudah ada.

### "Specificity Beats Creativity"
Model AI sudah sangat kreatif. Tugas prompt bukan menginspirasi, tapi **membatasi ruang eksplorasi ke arah yang tepat**. Semakin spesifik constraints yang diberikan, semakin predictable hasilnya.

### "Knowledge Separated from Logic"
Knowledge tentang setiap model AI (sintaks, parameters, best practices) disimpan terpisah dari logic workflow. Ini memungkinkan update knowledge tanpa harus edit workflow Opal.

### "Human-in-the-Loop"
Agent tidak langsung generate tanpa konfirmasi. Selalu ada checkpoint: klarifikasi input, preview output, konfirmasi model. Ini yang membuat hasilnya jauh lebih baik.

---

## Teknologi & Platform

| Komponen | Teknologi |
|----------|-----------|
| Workflow Builder | Google Opal |
| AI Engine | Gemini Flash (Agent Step) |
| Memory/Persistensi | Google Sheets |
| Knowledge Base | Markdown files (repo ini) |
| Routing | Opal Dynamic Routing |
| Interaction | Opal Interactive Chat |

---

## Skalabilitas

Sistem ini dirancang untuk **tumbuh tanpa friction**:

| Skenario | Cara Ekspansi |
|----------|---------------|
| Tambah cabang baru | Buat file baru di `02_branches/`, tambah path di router |
| Tambah model baru | Buat file baru di `03_models/`, tambah ke index |
| Update knowledge model | Edit file di `03_models/` — tidak perlu sentuh Opal |
| Perbarui system prompt | Edit `01_core/system_prompt.md` |

---

*Referensi: [flow.md](flow.md) untuk alur detail, [principles.md](principles.md) untuk keputusan desain*

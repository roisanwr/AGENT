# Alur Kerja End-to-End — AGENT

> Dokumen ini menjelaskan setiap langkah dalam alur kerja AGENT, dari input user hingga output prompt siap pakai.

---

## Diagram Alur Lengkap

```
                        ┌──────────────────────┐
                        │     USER INPUT       │
                        │  Tujuan + Konteks    │
                        └──────────┬───────────┘
                                   │
                    ┌──────────────▼───────────────┐
                    │     AGENT CLASSIFIER          │
                    │  (Opal Agent Step / Gemini)   │◄─────── Memory
                    │  • Baca + analisis input      │     (Google Sheets)
                    │  • Deteksi kategori           │
                    │  • Tanya balik jika ambigu    │
                    └──────────────┬───────────────┘
                                   │
                    ┌──────────────▼───────────────┐
                    │      DYNAMIC ROUTER           │
                    │  Pilih cabang berdasarkan     │
                    │  kategori yang terdeteksi     │
                    └──┬──┬──┬──┬──┬──┬────────────┘
                       │  │  │  │  │  │
            ┌──────────┘  │  │  │  │  └───────────────┐
            │    ┌────────┘  │  │  └──────────┐        │
            │    │    ┌──────┘  └──────┐       │        │
            ▼    ▼    ▼         ▼      ▼       ▼        ▼
         [IMG] [VID] [AUD]   [CODE] [PERSONA] [KONTEN] [+LAINNYA]
            │    │    │         │      │       │
            └────┴────┴─────────┴──────┴───────┘
                              │
                    ┌─────────▼───────────────────┐
                    │   SUB-AGENT PER CABANG       │
                    │  (Fixed Steps per kategori)  │
                    │  • Gali parameter spesifik   │
                    │  • Satu pertanyaan per step  │
                    │  • Validasi kelengkapan      │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │      MODEL SELECTOR          │
                    │  Agent rekomendasikan model  │
                    │  User konfirmasi/pilih       │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │     INTERACTIVE CHAT         │
                    │  Klarifikasi akhir           │
                    │  Pilihan gaya                │
                    └──────┬────────────┬──────────┘
                           │            │
                      (lanjut)       (revisi)
                           │            │
                           └────►──────►┘
                                  │
                    ┌─────────────▼───────────────┐
                    │      GENERATE PROMPT         │
                    │  Gabung semua parameter      │
                    │  Format sesuai model target  │
                    │  Tambah quality boosters     │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │     PREVIEW & ITERASI        │
                    │  Tampilkan draft prompt      │
                    │  User: Approve atau Revisi   │
                    └──────┬────────────┬──────────┘
                           │            │
                     (approve)       (revisi)──► kembali ke interactive chat
                           │
                    ┌──────▼────────────────────────┐
                    │        OUTPUT FINAL            │
                    │  • Prompt siap copy            │
                    │  • Simpan ke Google Sheets     │
                    │  • Update memory user          │
                    └───────────────────────────────┘
```

---

## Detail Setiap Step

### STEP 1 — User Input

**Platform:** Opal Input Step  
**Tujuan:** Mengumpulkan tujuan dan konteks dari user

**Field yang tersedia:**
| Field | Tipe | Keterangan |
|-------|------|-----------|
| `tujuan` | Text (wajib) | Apa yang ingin dibuat/capai |
| `konteks` | Text (opsional) | Referensi, contoh, atau detail tambahan |
| `target_model` | Text (opsional) | Jika user sudah tahu mau pakai model apa |

**Contoh input valid:**
```
"aku mau bikin gambar fantasy dark art buat thumbnail"
"buat prompt coding untuk sistem login dengan PHP Laravel"
"buatin musik upbeat untuk konten YouTube review gadget"
```

---

### STEP 2 — Agent Classifier

**Platform:** Opal Agent Step (Gemini Flash)  
**Tujuan:** Memahami intent user dan menentukan routing yang tepat

**System Prompt Classifier:**
> Lihat detail di [`01_core/classifier_rules.md`](../01_core/classifier_rules.md)

**Logika kerja:**
1. Baca input user + riwayat preferensi dari memory
2. Analisis kata kunci dan intent
3. Tentukan kategori: `gambar | video | audio | coding | persona | konten | ambigu`
4. Jika `ambigu`: tanya satu pertanyaan klarifikasi
5. Jika kategori jelas: langsung routing

**Contoh klarifikasi yang tepat:**
```
User: "buatkan konten tentang teknologi"
Agent: "Konten ini untuk platform apa — tulisan/artikel, script video, atau caption media sosial?"
```

**Yang TIDAK boleh dilakukan classifier:**
- Mengasumsikan kategori jika ada ambiguitas
- Mengajukan lebih dari satu pertanyaan sekaligus
- Langsung generate prompt tanpa routing ke sub-agent

---

### STEP 3 — Dynamic Router

**Platform:** Opal Dynamic Routing  
**Tujuan:** Mengarahkan ke cabang yang tepat

**Kondisi routing:**

| Kondisi | Cabang |
|---------|--------|
| Intent berkaitan dengan visual, gambar, ilustrasi, foto, artwork | `image` |
| Intent berkaitan dengan video, film, animasi, sinematik | `video` |
| Intent berkaitan dengan musik, audio, lagu, sound, suara | `audio` |
| Intent berkaitan dengan kode, program, script, debugging | `coding` |
| Intent berkaitan dengan karakter AI, chatbot, persona, asisten | `persona` |
| Intent berkaitan dengan tulisan, artikel, caption, copywriting, script | `content` |
| Tidak cocok dengan kondisi di atas | `fallback` *(tanya user)* |

> **Penting:** Routing adalah keputusan biner. Satu input hanya masuk ke satu cabang.

---

### STEP 4 — Sub-Agent per Cabang

**Platform:** Opal Fixed Steps  
**Tujuan:** Menggali parameter spesifik yang dibutuhkan untuk generate prompt berkualitas

**Aturan umum fixed steps:**
- Tanya **satu parameter per step**, tidak boleh bundling
- Selalu berikan pilihan/contoh untuk membantu user
- Jika parameter tidak dijawab, gunakan **default yang masuk akal** — jangan stop workflow
- Referensikan output step sebelumnya menggunakan `@nama_step`

**Lihat detail per cabang di:**
- [`02_branches/image.md`](../02_branches/image.md)
- [`02_branches/video.md`](../02_branches/video.md)
- [`02_branches/audio.md`](../02_branches/audio.md)
- [`02_branches/coding.md`](../02_branches/coding.md)
- [`02_branches/persona.md`](../02_branches/persona.md)
- [`02_branches/content.md`](../02_branches/content.md)

---

### STEP 5 — Model Selector

**Platform:** Opal Agent Step  
**Tujuan:** Memilih model AI target untuk generate prompt yang optimal

**Logika:**
1. Analisis kategori + parameter yang sudah dikumpulkan
2. Rekomendasikan 1-2 model yang paling cocok + alasannya
3. Tampilkan ke user untuk konfirmasi
4. Load knowledge spesifik model dari `03_models/`

**Contoh output model selector:**
```
Berdasarkan style yang kamu minta (dark fantasy, detailed illustration), 
saya rekomendasikan Midjourney v6 — paling kuat untuk style ini.
Alternatif: Flux jika kamu ingin detail tekstur lebih tinggi.
Pakai Midjourney v6 atau mau pilih lain?
```

---

### STEP 6 — Interactive Chat

**Platform:** Opal Interactive Chat  
**Tujuan:** Klarifikasi akhir dan penyesuaian halus sebelum generate

**Kapan digunakan:**
- Ada parameter opsional yang bisa meningkatkan kualitas
- User ingin eksplorasi beberapa variasi
- Ada konfirmasi model yang perlu dilakukan

**Aturan:**
- Maksimal 2-3 pertanyaan tambahan di tahap ini
- Jika user tidak merespons dalam satu giliran, gunakan default
- Selalu jelaskan **kenapa** pertanyaan itu penting

---

### STEP 7 — Generate Prompt

**Platform:** Opal Step (dengan knowledge dari `03_models/`)  
**Tujuan:** Menyusun prompt final yang berkualitas tinggi

**Yang harus dilakukan:**
1. Ambil semua parameter dari step sebelumnya via `@referensi`
2. Load struktur prompt yang benar untuk kategori ini
3. Apply format dan sintaks spesifik untuk model yang dipilih
4. Tambahkan quality boosters yang sesuai
5. Validasi format sebelum output (checklist dari `01_core/quality_gates.md`)

**Yang TIDAK boleh dilakukan:**
- Menebak parameter yang belum dikumpulkan
- Mengabaikan format spesifik model target
- Generate prompt tanpa melalui quality validation

---

### STEP 8 — Preview & Iterasi

**Platform:** Opal Interactive Chat  
**Tujuan:** User review dan approve prompt sebelum output final

**Pilihan yang diberikan ke user:**
- ✅ **Approve** — prompt diterima, lanjut ke output
- ✏️ **Revisi sebagian** — ubah elemen tertentu tanpa mulai dari awal
- 🔄 **Revisi besar** — kembali ke sub-agent untuk gali ulang parameter

**Format tampilan preview:**
```
--- DRAFT PROMPT KAMU ---
[prompt yang digenerate]

Target model: [nama model]
Kategori: [kategori]

Approve prompt ini atau ada yang mau diubah?
```

---

### STEP 9 — Output Final

**Platform:** Opal Output Step  
**Tujuan:** Mendistribusikan prompt final dan menyimpan ke memory

**Yang terjadi:**
1. Tampilkan prompt final yang sudah di-approve
2. Simpan ke Google Sheets dengan metadata:
   - User ID, timestamp, kategori, model, prompt, rating (jika ada)
3. Update memory user: model favorit, style preference, dll
4. Offer: "Mau buat prompt lain?" → kembali ke Step 1

---

## Memory Loop

Memory berfungsi di dua titik:

```
STEP 2 (Classifier) ← BACA memory → personalisasi routing
STEP 9 (Output)     → TULIS memory → simpan preferensi baru
```

**Data yang disimpan:**
- Kategori yang paling sering digunakan
- Model AI favorit per kategori
- Style/preference yang sering disebut
- Riwayat prompt yang dibuat (opsional)

---

*Lihat [`04_memory/schema.md`](../04_memory/schema.md) untuk detail struktur data*

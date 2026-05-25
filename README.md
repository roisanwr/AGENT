# AGENT — Opal Prompt Builder System

> **Sistem agent pembuat prompt berbasis Google Opal** dengan arsitektur multi-cabang yang modular, scalable, dan mudah dikembangkan.

---

## 🧭 Visi Proyek

Membangun sebuah **intelligent prompt engineering agent** yang mampu:
- Memahami tujuan user secara natural
- Mengklasifikasi dan merutekan permintaan ke cabang spesifik
- Menggali parameter yang tepat per kategori konten
- Menghasilkan prompt berkualitas tinggi yang disesuaikan dengan target model AI
- Belajar dari preferensi user lintas sesi

## 📁 Struktur Direktori

```
AGENT/
├── README.md                     ← Kamu ada di sini
│
├── 00_architecture/              ← Fondasi sistem
│   ├── overview.md               ← Gambaran besar arsitektur
│   ├── flow.md                   ← Alur kerja end-to-end
│   └── principles.md             ← Prinsip desain & keputusan teknis
│
├── 01_core/                      ← Rules inti agent
│   ├── system_prompt.md          ← System prompt master agent
│   ├── classifier_rules.md       ← Aturan klasifikasi & routing
│   └── quality_gates.md          ← Validasi kualitas output
│
├── 02_branches/                  ← Cabang-cabang spesifik
│   ├── _template.md              ← Template untuk cabang baru
│   ├── image.md                  ← Cabang: Gambar / Image
│   ├── video.md                  ← Cabang: Video
│   ├── audio.md                  ← Cabang: Audio / Musik
│   ├── coding.md                 ← Cabang: Coding / Dev
│   ├── persona.md                ← Cabang: Persona / Karakter AI
│   └── content.md                ← Cabang: Konten / Tulisan
│
├── 03_models/                    ← Knowledge spesifik per model AI
│   ├── _index.md                 ← Index semua model yang didukung
│   ├── image/
│   │   ├── midjourney.md         ← Midjourney v6+ rules
│   │   ├── dalle3.md             ← DALL-E 3 rules
│   │   ├── stable_diffusion.md   ← Stable Diffusion rules
│   │   ├── flux.md               ← Flux rules
│   │   └── ideogram.md           ← Ideogram rules
│   ├── video/
│   │   ├── veo.md                ← Google Veo 2/3 rules
│   │   ├── sora.md               ← OpenAI Sora rules
│   │   ├── kling.md              ← Kling rules
│   │   └── runway.md             ← Runway Gen rules
│   ├── audio/
│   │   ├── suno.md               ← Suno v4 rules
│   │   ├── udio.md               ← Udio rules
│   │   └── elevenlabs.md         ← ElevenLabs rules
│   └── text/
│       ├── claude.md             ← Claude (Anthropic) rules
│       ├── gpt.md                ← GPT-4o / GPT-5 rules
│       └── gemini.md             ← Gemini rules
│
├── 04_memory/                    ← Sistem memori & persistensi
│   ├── schema.md                 ← Struktur data Google Sheets
│   └── user_profile.md           ← Format profil user
│
├── 05_operations/                ← Panduan operasional
│   ├── how_to_add_branch.md      ← Cara menambah cabang baru
│   ├── how_to_add_model.md       ← Cara menambah model baru
│   ├── debugging.md              ← Panduan debug & troubleshoot
│   └── changelog.md              ← Log perubahan sistem
│
└── _assets/                      ← File pendukung (diagram, dll)
    ├── opal_prompt_agent_flow.svg
    ├── opal_implementation_plan.html
    ├── next_plan_model_specific.html
    └── prompt_quality_per_branch.html
```

## 🚀 Cara Menggunakan Dokumentasi Ini

1. **Mulai dari** [`00_architecture/overview.md`](00_architecture/overview.md) untuk memahami gambaran besar
2. **Baca** [`00_architecture/flow.md`](00_architecture/flow.md) untuk memahami alur kerja
3. **Implementasikan** rules dari `01_core/` ke dalam Opal agent step
4. **Pilih cabang** yang relevan dari `02_branches/` untuk sub-agent
5. **Tambahkan knowledge** model dari `03_models/` ke generate step
6. **Setup memory** menggunakan schema di `04_memory/`
7. **Gunakan panduan** di `05_operations/` untuk maintenance

## ⚡ Quick Start: Alur Utama

```
User Input
    ↓
Discovery Stage (Interactive Chat)
    ↓
Agent Classifier (Silent Router)
    ↓
Dynamic Router → [Gambar | Video | Audio | Coding | Persona | Konten | Fallback]
    ↓
Sub-Agent per Cabang (Fixed Steps)
    ↓
Model Selector
    ↓
Interactive Chat (Klarifikasi)
    ↓
Generate Prompt
    ↓
Preview & Iterasi
    ↓
Output: Prompt Siap Pakai
```

## 📌 Prinsip Utama

| Prinsip | Penjelasan |
|--------|-----------|
| **Modular** | Tiap cabang independen, tidak saling mempengaruhi |
| **Scalable** | Tambah cabang baru tanpa ubah yang sudah ada |
| **Specific** | Satu prompt per tujuan, bukan one-size-fits-all |
| **Iterative** | Ada loop feedback sebelum output final |
| **Persistent** | Preferensi user tersimpan lintas sesi |

---

*Terakhir diperbarui: Mei 2026 | Versi: 1.0.0*

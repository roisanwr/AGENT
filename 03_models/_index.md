# Index Model AI — AGENT

> Direktori semua model AI yang knowledge-nya sudah terdokumentasi dalam sistem ini.
> Update dokumen ini setiap kali model baru ditambahkan.

---

## Model yang Didukung

### 🖼️ Kategori: Gambar

| Model | File | Status | Terakhir Update |
|-------|------|--------|----------------|
| Midjourney v6+ | [`image/midjourney.md`](image/midjourney.md) | ✅ Aktif | Mei 2026 |
| DALL-E 3 | [`image/dalle3.md`](image/dalle3.md) | ✅ Aktif | Mei 2026 |
| Stable Diffusion (SDXL/SD3) | [`image/stable_diffusion.md`](image/stable_diffusion.md) | ✅ Aktif | Mei 2026 |
| Flux | [`image/flux.md`](image/flux.md) | ✅ Aktif | Mei 2026 |
| Ideogram v2 | [`image/ideogram.md`](image/ideogram.md) | ✅ Aktif | Mei 2026 |
| Adobe Firefly | *Belum ada* | 📋 Planned | - |

### 🎬 Kategori: Video

| Model | File | Status | Terakhir Update |
|-------|------|--------|----------------|
| Google Veo 2/3 | [`video/veo.md`](video/veo.md) | ✅ Aktif | Mei 2026 |
| OpenAI Sora | [`video/sora.md`](video/sora.md) | ✅ Aktif | Mei 2026 |
| Kling | [`video/kling.md`](video/kling.md) | ✅ Aktif | Mei 2026 |
| Runway Gen-3 Alpha | [`video/runway.md`](video/runway.md) | ✅ Aktif | Mei 2026 |
| Pika 2.0 | *Belum ada* | 📋 Planned | - |

### 🎵 Kategori: Audio

| Model | File | Status | Terakhir Update |
|-------|------|--------|----------------|
| Suno v4 | [`audio/suno.md`](audio/suno.md) | ✅ Aktif | Mei 2026 |
| Udio | [`audio/udio.md`](audio/udio.md) | ✅ Aktif | Mei 2026 |
| ElevenLabs | [`audio/elevenlabs.md`](audio/elevenlabs.md) | ✅ Aktif | Mei 2026 |
| MusicGen (Meta) | *Belum ada* | 📋 Planned | - |

### 💬 Kategori: Teks / Coding

| Model | File | Status | Terakhir Update |
|-------|------|--------|----------------|
| Claude 3.7 Sonnet | [`text/claude.md`](text/claude.md) | ✅ Aktif | Mei 2026 |
| GPT-4o / GPT-5 | [`text/gpt.md`](text/gpt.md) | ✅ Aktif | Mei 2026 |
| Gemini 3 (Flash/Pro) | [`text/gemini.md`](text/gemini.md) | ✅ Aktif | Mei 2026 |

---

## Cara Memilih Model yang Tepat

### Untuk Gambar

| Kebutuhan | Model Terbaik |
|-----------|--------------|
| Style artistik, anime, fantasy | Midjourney v6 |
| Foto realistis, akurasi tinggi | DALL-E 3 atau Flux |
| Kontrol penuh, custom LoRA | Stable Diffusion |
| Teks di dalam gambar | Ideogram v2 |
| Detail material/tekstur | Flux |

### Untuk Video

| Kebutuhan | Model Terbaik |
|-----------|--------------|
| Kualitas sinematik tertinggi | Veo 3 |
| Fisika realistis, gerak natural | Sora |
| Video dari gambar referensi | Kling |
| Efek visual kreatif | Runway Gen-3 |

### Untuk Audio

| Kebutuhan | Model Terbaik |
|-----------|--------------|
| Full song dengan lirik | Suno v4 |
| Musik instrumental | Udio atau MusicGen |
| Voice/voiceover | ElevenLabs |
| Experimental/avant-garde | Udio |

### Untuk Teks/Coding

| Kebutuhan | Model Terbaik |
|-----------|--------------|
| Long-form writing, persona | Claude |
| Code generation, reasoning | GPT-4o / GPT-5 |
| Multimodal, Google Workspace | Gemini |

---

## Cara Menambah Model Baru

1. Buat file baru di direktori yang sesuai: `03_models/[kategori]/[nama_model].md`
2. Gunakan struktur yang sama dengan file model yang sudah ada
3. Isi semua field: sintaks, karakteristik, best practices, hal yang dihindari
4. Tambahkan entri baru ke tabel di atas
5. Update `02_branches/[kategori].md` dengan menambahkan model ke pilihan

> Lihat panduan lengkap: [`../05_operations/how_to_add_model.md`](../05_operations/how_to_add_model.md)

---

## Knowledge Versioning

Setiap file model memiliki `version` dan `last_updated`. Ini penting karena:
- Model AI sering update fitur dan sintaks
- Knowledge yang outdated bisa menghasilkan prompt yang tidak optimal
- Target review: setiap 3 bulan atau saat ada update model signifikan

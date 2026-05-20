# Changelog — AGENT

> Log semua perubahan pada sistem AGENT. Setiap entri harus mencakup tanggal, jenis perubahan, dan dampak.

---

## Format Entri

```markdown
### [YYYY-MM-DD] — [Jenis: TAMBAH/UBAH/HAPUS/FIX]
**File yang Berubah:** [daftar file]
**Deskripsi:** [apa yang berubah dan mengapa]
**Impact:** [apa yang terpengaruh oleh perubahan ini]
**Oleh:** [siapa yang melakukan]
```

---

## v1.0.0 — 2026-05-20 — Initial Release

### [2026-05-20] — TAMBAH: Struktur dasar sistem
**File yang Berubah:** Semua file (initial creation)

**Deskripsi:**
Pembuatan struktur dasar sistem AGENT dengan:
- Arsitektur 5-lapisan (Input → Classifier → Router → Sub-agent → Generate → Output)
- 6 cabang: Gambar, Video, Audio, Coding, Persona, Konten
- Knowledge base untuk model utama: Midjourney, DALL-E 3, Veo, Suno, Claude
- Memory schema menggunakan Google Sheets
- Panduan operasional: tambah cabang, debugging

**Cabang yang tersedia:**
- ✅ `image.md` — Cabang gambar
- ✅ `video.md` — Cabang video
- ✅ `audio.md` — Cabang audio
- ✅ `coding.md` — Cabang coding
- ✅ `persona.md` — Cabang persona
- ✅ `content.md` — Cabang konten

**Model knowledge yang tersedia:**
- ✅ `midjourney.md` — Midjourney v6+
- ✅ `dalle3.md` — DALL-E 3
- ✅ `suno.md` — Suno v4
- ✅ `claude.md` — Claude 3.7
- ✅ `veo.md` — Google Veo 2/3
- 📋 Planned: stable_diffusion, flux, ideogram, sora, kling, runway, udio, elevenlabs, gpt, gemini

**Impact:** N/A — initial release

**Oleh:** Antigravity

---

### [2026-05-20] — UBAH: Penyelarasan Model Internal Opal ke Gemini 3
**File yang Berubah:** 
- `c:\AGENT\opalflow.md`
- `c:\AGENT\03_models\_index.md`
- `c:\AGENT\01_core\system_prompt.md`

**Deskripsi:**
Menyelaraskan nama model internal Opal yang didokumentasikan dengan kondisi aktual platform terbaru (Gemini 3 Flash & Gemini 3.1 Pro). Menegaskan batas cakupan project AGENT bahwa workflow prompt builder hanya berjalan menggunakan engine teks Gemini 3, sementara model visual/audio internal Opal lainnya (Nano Banana, Lyria 2, AudioLM, Veo) berada di luar eksekusi workflow pembentuk prompt ini.

**Impact:**
- Dokumentasi teknis alur Opal (`opalflow.md`) kini akurat sesuai runtime model di dashboard Opal.
- Penentuan model di `system_prompt.md` dan index models selaras menggunakan Gemini 3.

---


## Template untuk Entri Berikutnya

Gunakan template ini untuk entri changelog berikutnya:

```markdown
### [YYYY-MM-DD] — [TAMBAH/UBAH/HAPUS/FIX]: [Judul singkat]
**File yang Berubah:** 
- `path/ke/file.md`

**Deskripsi:**
[Penjelasan detail apa yang berubah dan mengapa]

**Impact:**
- [Dampak 1]
- [Dampak 2]

**Oleh:** [nama/inisial]
```

---

## Roadmap

| Feature | Status | Target |
|---------|--------|--------|
| 6 cabang dasar | ✅ Done | v1.0 |
| Knowledge Midjourney, DALL-E, Suno, Claude, Veo | ✅ Done | v1.0 |
| Knowledge: Stable Diffusion, Flux, Ideogram | 📋 Planned | v1.1 |
| Knowledge: Sora, Kling, Runway | 📋 Planned | v1.1 |
| Knowledge: Udio, ElevenLabs | 📋 Planned | v1.1 |
| Knowledge: GPT-4o/5, Gemini | 📋 Planned | v1.1 |
| Cabang: Presentasi | 📋 Planned | v1.2 |
| Cabang: Iklan / Advertising | 📋 Planned | v1.2 |
| Cabang: Email Marketing | 📋 Planned | v1.2 |
| Memory integration testing | 📋 Planned | v1.1 |
| Feedback loop implementation | 📋 Planned | v1.2 |
| Multi-user support | 💭 Considering | v2.0 |

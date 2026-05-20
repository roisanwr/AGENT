# Cara Menambah Model AI Baru — Panduan Operasional

> Panduan untuk mendokumentasikan knowledge model AI baru tanpa mengganggu sistem yang sudah ada.

---

## Prinsip

Menambah model baru = menambah file knowledge baru.  
Tidak ada yang perlu diubah di Opal.  
Yang perlu diperbarui: file knowledge, index, dan pilihan di cabang terkait.

---

## Checklist

```
□ Tentukan kategori model (image/video/audio/text)
□ Research karakteristik model terbaru
□ Buat file 03_models/[kategori]/[nama_model].md
□ Update 03_models/_index.md
□ Update pilihan model di 02_branches/[kategori].md
□ Update knowledge yang dipakai di generate step Opal
```

---

## Struktur File Knowledge Model

Setiap file model harus memiliki bagian:

```markdown
# Knowledge: [Nama Model]

| Metadata table |

## Karakteristik Model
- Kekuatan
- Kelemahan
- Pendekatan fundamental

## Sintaks & Parameter (jika ada)
## Format Prompt Optimal
## Struktur Prompt
## Best Practices (DO/DON'T)
## Contoh Prompt per Use Case
## Changelog Model
```

---

## Cara Menentukan "Kekuatan" Model

Research dari:
1. Official documentation / release notes model
2. Community benchmarks dan perbandingan
3. Prompt engineering guides dari platform resmi
4. Uji sendiri dengan berbagai input

---

## Update Knowledge yang Sudah Ada

Ketika model update versi besar:
1. Cek release notes resmi
2. Update bagian yang berubah di file knowledge
3. Update versi dokumen dan tanggal `Terakhir Update`
4. Tambahkan entri ke bagian `Changelog Model` di file tersebut
5. Catat di `05_operations/changelog.md`

---

## Naming Convention

| Kategori | Direktori | Nama File |
|----------|-----------|-----------|
| Image | `03_models/image/` | `midjourney.md`, `dalle3.md`, `flux.md` |
| Video | `03_models/video/` | `veo.md`, `sora.md`, `kling.md` |
| Audio | `03_models/audio/` | `suno.md`, `udio.md` |
| Text/Code | `03_models/text/` | `claude.md`, `gpt.md`, `gemini.md` |

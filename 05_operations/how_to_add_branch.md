# Cara Menambah Cabang Baru — Panduan Operasional

> Panduan step-by-step untuk menambahkan cabang prompt baru ke sistem AGENT tanpa merusak yang sudah ada.

---

## Prinsip Dasar

> **Menambah cabang baru = menambah modul baru.**  
> Tidak ada yang perlu diubah di cabang yang sudah ada.  
> Yang perlu diperbarui hanya: classifier, router, dan dokumen index.

---

## Checklist Lengkap

```
Fase 1: Persiapan
□ Tentukan nama cabang (lowercase, bahasa Inggris)
□ Identifikasi kata kunci deteksi (kuat + pendukung)
□ Petakan ambiguitas dengan cabang yang sudah ada
□ Tentukan target model AI yang relevan
□ Tentukan minimal 3 parameter yang akan digali

Fase 2: Dokumentasi
□ Buat file 02_branches/[nama].md (copy dari _template.md)
□ Isi semua field di template
□ Buat file knowledge model di 03_models/ jika model baru
□ Update 03_models/_index.md

Fase 3: Konfigurasi Sistem
□ Update 01_core/classifier_rules.md (tambah keyword baru)
□ Update bagian "KATEGORI YANG TERSEDIA" di 01_core/system_prompt.md
□ Tambahkan kondisi routing baru di Opal Dynamic Router
□ Buat sub-agent baru di Opal untuk cabang ini

Fase 4: Testing
□ Uji minimal 5 input yang seharusnya masuk cabang ini
□ Uji 3 input ambigu dengan cabang yang sudah ada
□ Uji fallback jika cabang ini tidak match
□ Verifikasi bahwa cabang lain tidak terganggu
```

---

## Fase 1: Persiapan

### Tentukan Nama Cabang
```
Format: kata tunggal, lowercase, bahasa Inggris
Contoh yang baik: presentation, education, advertising, email
Contoh yang buruk: Presentasi, content_script, NEW_BRANCH
```

### Identifikasi Kata Kunci
Buat daftar:
1. **Kata kunci kuat** — hampir pasti menunjukkan cabang ini
2. **Kata kunci pendukung** — membantu konfirmasi
3. **Frasa umum** — kalimat lengkap yang sering diucapkan user
4. **Potensi ambigu** — kata yang bisa masuk cabang lain

### Parameter yang Akan Digali
Tentukan 3-6 parameter utama dengan urutan dari yang paling kritis:
1. Parameter yang selalu dibutuhkan (WAJIB)
2. Parameter yang sangat membantu kualitas (PENTING)
3. Parameter opsional yang meningkatkan presisi

---

## Fase 2: Dokumentasi

### Buat File Cabang

1. Copy `02_branches/_template.md`
2. Rename menjadi `02_branches/[nama_cabang].md`
3. Isi semua field `[ISIAN]` dalam template
4. Hapus section "Template" di bagian bawah setelah selesai

### Update Index Model

Jika ada model AI baru yang perlu di-cover:
1. Buat `03_models/[kategori]/[nama_model].md`
2. Isi dengan struktur sama seperti file model yang sudah ada
3. Tambahkan entri ke `03_models/_index.md`

### Contoh: Menambah Cabang "Presentation"

**02_branches/presentation.md** (ringkasan):
```markdown
| Nama Cabang | presentation |
| Label | Presentasi / Slide |
| Target Model | GPT-4o, Claude, Gamma |

Kata kunci kuat: presentasi, slide, PowerPoint, keynote, deck
Parameter: topik, audiens, jumlah slide, tone, struktur, visual preference
```

---

## Fase 3: Konfigurasi Sistem

### Update classifier_rules.md

Tambahkan ke file `01_core/classifier_rules.md`:

**Di bagian "Peta Kata Kunci per Kategori":**
```markdown
### 🎯 PRESENTATION

**Kata kunci kuat:**
```
presentasi, slide, deck, PowerPoint, Keynote, slideshow,
pitch deck, investor deck, presentation, slides
```

**Kata kunci pendukung:**
```
...
```
```

**Di bagian "Matriks Ambiguitas":**
```markdown
| "konten presentasi" | PRESENTATION atau KONTEN | Tanya: "Slide presentasi atau artikel/tulisan?" |
```

**Di bagian "Testing Routing":**
```markdown
| "buat pitch deck untuk startup" | PRESENTATION | ⬜ belum diuji |
```

### Update system_prompt.md

Di bagian `## KATEGORI YANG TERSEDIA`, tambahkan:
```
- PRESENTATION: Slide dan presentasi — PowerPoint, Keynote, Google Slides
```

### Konfigurasi Opal Dynamic Router

Tambahkan jalur baru:
```
Jika output classifier mengandung "KATEGORI: presentation"
→ Rutekan ke sub-agent Presentation
```

### Buat Sub-Agent di Opal

1. Tambahkan **Agent Step** baru atau **Fixed Steps** baru di workflow
2. Label: `presentation_[nama_step]` (ikuti naming convention)
3. Copy system prompt dari bagian `## System Prompt Sub-Agent` di file cabang
4. Hubungkan dari Dynamic Router

---

## Fase 4: Testing

### Test Cases yang Harus Berhasil

Sebelum launch, uji semua ini:

**Input yang harus masuk cabang baru:**
```
1. "buat presentasi untuk meeting klien besok"
2. "pitch deck untuk investor startup AI"
3. "slide keynote dengan 20 halaman tentang..."
4. "PowerPoint untuk training karyawan baru"
5. "deck presentasi dengan tema [X]"
```

**Input ambigu yang harus minta klarifikasi:**
```
1. "materi tentang AI" → tanya: artikel atau presentasi?
2. "konten untuk meeting" → tanya: slide presentasi atau dokumen?
```

**Input yang TIDAK boleh masuk cabang ini:**
```
1. "artikel blog tentang presentasi" → harus ke KONTEN
2. "video tutorial cara presentasi" → harus ke VIDEO
```

### Verifikasi Cabang Lain Tidak Terganggu

Jalankan ulang semua test cases dari testing routing yang ada di `01_core/classifier_rules.md`. Pastikan tidak ada regresi.

---

## Troubleshooting Umum

### Problem: Cabang baru tidak pernah dipilih
**Kemungkinan penyebab:**
1. Kata kunci tidak ditambahkan ke classifier
2. Kondisi routing di Opal tidak ditambahkan
3. Kata kunci terlalu mirip dengan cabang yang sudah ada

**Solusi:**
1. Cek `01_core/classifier_rules.md` — pastikan kata kunci sudah ada
2. Cek Opal Dynamic Router — pastikan kondisi baru sudah disave
3. Perkuat kata kunci — gunakan yang lebih unik

### Problem: Cabang baru menyerobot routing cabang lain
**Kemungkinan penyebab:**
Kata kunci terlalu generik dan overlap dengan cabang yang sudah ada.

**Solusi:**
1. Identifikasi kata kunci yang overlap
2. Pindahkan ke ambiguity matrix
3. Tambahkan klarifikasi di classifier untuk disambiguasi

### Problem: Sub-agent tidak mengumpulkan parameter dengan benar
**Kemungkinan penyebab:**
System prompt sub-agent tidak jelas atau context dari classifier hilang.

**Solusi:**
1. Pastikan `@classifier_output` di-referensikan dalam sub-agent
2. Periksa system prompt sub-agent — uji dengan input manual
3. Tambahkan instruksi lebih eksplisit tentang urutan step

---

## Cadence Review

| Milestone | Action |
|-----------|--------|
| Setelah launch | Monitor routing accuracy selama 1 minggu |
| Setelah 20 penggunaan | Review output quality, update knowledge jika perlu |
| Setiap 3 bulan | Review apakah parameter yang digali masih relevan |
| Saat model target update | Update file di `03_models/` |

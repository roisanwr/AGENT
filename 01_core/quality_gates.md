# Quality Gates — Validasi Kualitas Output

> Checklist validasi yang harus dijalankan oleh generate step **sebelum** prompt dikirim ke user untuk preview.

---

## Apa Itu Quality Gate?

Quality gate adalah checkpoint otomatis yang memastikan prompt yang dihasilkan memenuhi standar minimum kualitas. Jika ada gate yang gagal, generate step harus:
1. Mencoba mengisi sendiri dengan default yang masuk akal
2. Atau kembali ke interactive chat untuk meminta informasi yang kurang

---

## Gate Universal (Berlaku di Semua Cabang)

### Gate 1: Kelengkapan Parameter Wajib ✅
```
CEKLIS:
□ Semua parameter WAJIB sudah terisi (bukan null/kosong)
□ Parameter yang kosong sudah diisi dengan default yang sesuai
□ Default yang digunakan sudah diberitahu ke user
```

### Gate 2: Konsistensi Konteks ✅
```
CEKLIS:
□ Semua parameter saling konsisten (tidak kontradiksi)
□ Style yang dipilih kompatibel dengan model target
□ Tidak ada elemen yang saling bertentangan dalam prompt
```

### Gate 3: Target Model Terdefinisi ✅
```
CEKLIS:
□ Target model sudah dipilih (tidak boleh "undefined" atau "semua")
□ Format prompt sesuai dengan sintaks model yang dipilih
□ Knowledge spesifik model sudah di-load
```

### Gate 4: Panjang Prompt Wajar ✅
```
CEKLIS:
□ Prompt tidak terlalu pendek (kurang dari 10 kata = terlalu umum)
□ Prompt tidak terlalu panjang (sesuai limit model target)
□ Jika terlalu panjang: prioritaskan elemen terpenting, beri tahu user
```

### Gate 5: Tidak Ada Elemen Berbahaya ✅
```
CEKLIS:
□ Tidak ada permintaan konten yang melanggar kebijakan model target
□ Tidak ada elemen yang akan ditolak oleh safety filter model
□ Jika ada: informasikan user dan tawarkan alternatif aman
```

---

## Gate Spesifik per Cabang

### 🖼️ Cabang GAMBAR

```
PARAMETER WAJIB yang harus ada:
□ Subjek utama (jelas dan spesifik)
□ Style/medium visual (oil painting, photography, illustration, dll)
□ Aspect ratio / komposisi

PARAMETER SANGAT DIREKOMENDASIKAN:
□ Lighting (tanpa ini, model akan pilih default yang random)
□ Mood/atmosphere
□ Quality boosters di akhir

VALIDASI FORMAT:
□ Jika Midjourney: ada --ar, tidak ada kata yang "dilarang" MJ
□ Jika DALL-E 3: format natural descriptive, bukan keyword stack
□ Jika SD: ada positive prompt + negative prompt terpisah
□ Jika Flux: deskripsi material/tekstur sudah ada
```

### 🎬 Cabang VIDEO

```
PARAMETER WAJIB:
□ Camera movement / shot type (PALING SERING DILUPAKAN)
□ Subjek + aksi (sequential, bukan deskripsi statis)
□ Durasi / pace

PARAMETER SANGAT DIREKOMENDASIKAN:
□ Tone visual
□ Setting/latar

VALIDASI FORMAT:
□ Prompt mendeskripsikan GERAK dan WAKTU, bukan deskripsi statis
□ Jika Veo: ada camera movement descriptor
□ Jika Sora: ada physics descriptor untuk objek bergerak
□ Satu prompt = satu continuous shot (tidak boleh multi-scene)
```

### 🎵 Cabang AUDIO

```
PARAMETER WAJIB:
□ Genre + sub-genre (spesifik, bukan hanya "elektronik")
□ Mood/energi
□ Instrumental atau vokal

PARAMETER SANGAT DIREKOMENDASIKAN:
□ Instrumen utama
□ BPM atau keterangan tempo
□ Durasi target

VALIDASI FORMAT:
□ Jika Suno: style prompt < 200 karakter
□ Jika Suno dengan lirik: lirik menggunakan section tags [Verse][Chorus][Bridge]
□ Jika Udio: ada tag genre yang valid untuk Udio
```

### 💻 Cabang CODING

```
PARAMETER WAJIB:
□ Bahasa pemrograman + versi
□ Framework (jika relevan)
□ Tujuan fungsi/program (satu kalimat jelas)
□ Input dan output yang diharapkan

PARAMETER SANGAT DIREKOMENDASIKAN:
□ Constraint (batasan yang harus dipenuhi)
□ Gaya kode yang diinginkan
□ Konteks proyek jika ini bagian dari sistem lebih besar

VALIDASI FORMAT:
□ Prompt menggunakan bahasa yang presisi dan teknis
□ Ada contoh input/output konkret (bukan abstrak)
□ Tidak ada instruksi yang kontradiksi
```

### 🤖 Cabang PERSONA

```
PARAMETER WAJIB:
□ Identitas karakter (nama, latar singkat)
□ Cara bicara dan tone
□ Domain keahlian

PARAMETER SANGAT DIREKOMENDASIKAN:
□ Batasan eksplisit (apa yang TIDAK akan dilakukan)
□ Minimal 1-2 contoh Q&A ideal
□ Konteks penggunaan

VALIDASI FORMAT:
□ Ada contoh respons konkret (bukan hanya deskripsi)
□ Kepribadian tidak kontradiksi (misal: "formal tapi santai" tanpa aturan kapan)
□ Batasan sudah didefinisikan
```

### 📝 Cabang KONTEN

```
PARAMETER WAJIB:
□ Format + panjang eksplisit
□ Audiens yang spesifik
□ Tujuan konten (edukasi/konversi/awareness/dll)

PARAMETER SANGAT DIREKOMENDASIKAN:
□ Platform distribusi
□ Tone + gaya bahasa spesifik
□ Hal-hal yang harus dihindari

VALIDASI FORMAT:
□ Tone tidak terlalu abstrak ("profesional" harus dijelaskan seperti apa)
□ Ada instruksi tentang angle/hook jika konten pemasaran
□ Platform disebutkan (gaya Instagram ≠ LinkedIn)
```

---

## Tindakan jika Gate Gagal

| Kondisi | Tindakan |
|---------|----------|
| Parameter wajib kosong | Isi dengan default + beri tahu user |
| Parameter kontradiksi | Tanya user mana yang diprioritaskan |
| Target model undefined | Minta user pilih dari rekomendasi |
| Prompt terlalu panjang | Potong & prioritaskan elemen terpenting |
| Elemen berpotensi ditolak | Informasikan & tawarkan versi aman |
| Format tidak sesuai model | Auto-format sesuai knowledge model |

---

## Format Laporan Quality Check ke User

Jika ada gate yang perlu perhatian user, sampaikan dalam format ini:
```
⚠️ Catatan sebelum generate:
- [Parameter X] tidak disebutkan, saya gunakan default: [nilai default]
- [Parameter Y] kontradiksi dengan [Z], saya prioritaskan [Z]

Lanjut generate dengan kondisi ini? Atau mau ubah dulu?
```

---

*Dokumen ini adalah living document — update setiap kali ada pola kegagalan baru yang ditemukan*

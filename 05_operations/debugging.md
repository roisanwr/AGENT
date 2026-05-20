# Panduan Debug & Troubleshooting — AGENT

> Panduan untuk mendiagnosis dan memperbaiki masalah yang umum terjadi pada sistem AGENT.

---

## Anatomi Masalah yang Umum

Masalah di AGENT umumnya terjadi di salah satu dari empat titik:

```
[INPUT] → [ROUTING] → [PARAMETER GATHERING] → [GENERATE]
  1️⃣         2️⃣              3️⃣                 4️⃣
```

1. **Input tidak dipahami** — classifier salah mengklasifikasi
2. **Routing salah** — masuk ke cabang yang tidak tepat
3. **Parameter kurang** — sub-agent tidak menggali cukup
4. **Generate buruk** — output prompt tidak berkualitas

---

## Tanda-Tanda Masalah per Titik

### Titik 1: Input / Classifier

| Gejala | Indikasi |
|--------|----------|
| Classifier tidak merespons atau terhenti | Sistem prompt classifier mungkin error |
| Classifier selalu tanya balik walau input sudah jelas | Kondisi klasifikasi terlalu strict |
| Classifier tidak pernah tanya balik walau ambigu | Kondisi klasifikasi terlalu permisif |
| Routing ke cabang yang jelas salah | Keyword conflict antar cabang |

### Titik 2: Routing

| Gejala | Indikasi |
|--------|----------|
| Fallback terus menerus | Kondisi routing tidak menangkap output classifier |
| Selalu routing ke cabang yang sama | Kondisi satu cabang terlalu broad |
| Error saat routing | Kondisi Opal tidak tersimpan dengan benar |

### Titik 3: Parameter Gathering

| Gejala | Indikasi |
|--------|----------|
| Sub-agent mengajukan terlalu banyak pertanyaan sekaligus | System prompt tidak mengikuti aturan one-question-at-a-time |
| Sub-agent skip parameter penting | Fixed steps tidak dikonfigurasi dengan benar |
| Sub-agent langsung generate tanpa tanya | Routing ke generate step terlalu cepat |
| Parameter yang dikumpulkan tidak muncul di generate | `@referensi` ke step sebelumnya tidak dikonfigurasi |

### Titik 4: Generate

| Gejala | Indikasi |
|--------|----------|
| Prompt terlalu generik/tidak spesifik | Parameter belum lengkap saat generate |
| Format prompt salah untuk model yang dipilih | Knowledge model tidak di-load |
| Prompt terlalu panjang | Tidak ada pemotongan / limit |
| Elemen prompt kontradiksi | Quality gate tidak berjalan |

---

## Prosedur Debugging

### Step 1: Isolasi Titik Masalah

Jalankan input test secara manual dan perhatikan di mana alur berhenti atau berperilaku aneh:

```
Test input: "buat gambar dragon realistic"

Expected:
✅ Classifier → KATEGORI: gambar, KONFIDENSSI: tinggi
✅ Router → masuk ke sub-agent gambar
✅ Sub-agent → tanya subjek, style, mood, ratio
✅ Model selector → rekomendasikan Midjourney atau Flux
✅ Generate → prompt berkualitas dengan format yang benar

Actual:
❓ Di mana alur mulai menyimpang?
```

### Step 2: Cek Opal Debug Mode

Opal memiliki fitur untuk melihat log setiap step:
1. Buka workflow di Opal
2. Aktifkan debug/trace mode
3. Jalankan input yang bermasalah
4. Perhatikan output di setiap step

### Step 3: Isolasi System Prompt

Uji system prompt classifier dan sub-agent secara terpisah di Gemini / Claude langsung:
```
1. Buka Gemini atau Claude
2. Paste system prompt yang ingin diuji
3. Kirim input yang bermasalah
4. Lihat apakah outputnya sesuai harapan
5. Jika tidak → masalah ada di system prompt
6. Jika ya → masalah ada di konfigurasi Opal
```

### Step 4: Cek Referensi antar Step

Pastikan setiap step yang membutuhkan output step sebelumnya sudah menggunakan referensi yang benar:
```
Contoh yang benar di Opal:
"Berdasarkan @image_subject yang dikumpulkan sebelumnya..."

Yang salah:
"Berdasarkan input user sebelumnya..." (terlalu ambigu)
```

---

## Kasus Spesifik & Solusinya

### Kasus 1: Routing Salah

**Gejala:** User minta "buat thumbnail YouTube" tapi masuk ke cabang VIDEO, bukan GAMBAR.

**Diagnosis:** Kata "YouTube" di-associate dengan VIDEO.

**Solusi:**
1. Buka `01_core/classifier_rules.md`
2. Di bagian ambiguity matrix, tambahkan:
   ```
   | "thumbnail YouTube" | GAMBAR (bukan VIDEO) | Thumbnail adalah output statis |
   ```
3. Update system prompt classifier untuk memperkuat ini:
   ```
   CATATAN: "thumbnail" selalu = GAMBAR, bukan VIDEO. 
   Thumbnail adalah output statis.
   ```

---

### Kasus 2: Output Generik

**Gejala:** Prompt yang dihasilkan terlalu generik, tidak ada parameter spesifik.

**Diagnosis:** Parameter yang dikumpulkan sub-agent tidak masuk ke generate step.

**Langkah debugging:**
1. Cek apakah sub-agent menghasilkan output dalam format yang benar:
   ```
   PARAMETER_TERKUMPUL:
   - subjek: ...
   - style: ...
   ```
2. Cek apakah generate step me-referensikan `@subagent_output`
3. Cek apakah system prompt generate step memiliki instruksi untuk menggunakan parameter

**Solusi:** Pastikan format output sub-agent konsisten dan generate step me-referensikannya.

---

### Kasus 3: Classifier Selalu Tanya Balik

**Gejala:** Hampir semua input memicu klarifikasi, bahkan yang sudah jelas.

**Diagnosis:** Threshold untuk "ambigu" di classifier terlalu rendah.

**Solusi:**
1. Edit system prompt classifier di `01_core/system_prompt.md`
2. Perkuat bagian "ATURAN KLARIFIKASI":
   ```
   Tanya balik HANYA jika:
   - Probabilitas dua kategori selisihnya kurang dari 20%
   - TIDAK ADA kata kunci kuat yang terdeteksi
   ```
3. Tambahkan contoh input yang TIDAK perlu klarifikasi:
   ```
   "buat gambar panda" → langsung GAMBAR, tidak perlu tanya
   ```

---

### Kasus 4: Memory Tidak Berfungsi

**Gejala:** Preferensi user tidak tersimpan atau tidak dibaca antar sesi.

**Diagnosis:** Koneksi ke Google Sheets bermasalah atau format data salah.

**Langkah debugging:**
1. Cek apakah Google Sheets masih accessible (permissions tidak expired)
2. Cek apakah user_id konsisten antar sesi
3. Cek apakah kolom di Sheets sesuai dengan schema di `04_memory/schema.md`
4. Uji manual: write test entry ke Sheets, cek apakah readable dari Opal

---

## Log Format

Ketika menemukan bug, catat dengan format ini untuk dimasukkan ke changelog:

```markdown
## Bug #[nomor]
**Tanggal:** [tanggal]
**Titik Masalah:** [Input/Routing/Parameter/Generate]
**Gejala:** [apa yang terjadi]
**Root Cause:** [mengapa terjadi]
**Solusi:** [apa yang diubah]
**Files Updated:** [file yang diubah]
**Test Cases Added:** [test case baru yang ditambahkan]
```

---

## Preventive Measures

Hal-hal yang bisa mencegah bug sebelum terjadi:

1. **Testing sebelum launch** — jalankan semua test cases di `classifier_rules.md` setiap kali ada perubahan
2. **Review berkala** — cek apakah model AI target sudah update sintaks/parameter
3. **Monitor feedback** — cek sheet `feedback` di Google Sheets secara rutin
4. **Changelog discipline** — setiap perubahan harus dicatat di `05_operations/changelog.md`

# Panduan Layer `01_core` — Cara Kerja & Instalasi di Google Opal

> **Tujuan dokumen ini:** Menjelaskan peran setiap file di folder `01_core` dan panduan implementasi teknis yang detail dan akurat di canvas Google Opal.

---

## 🧠 Apa itu Layer `01_core`?

`01_core` adalah lapisan pertama dan "otak utama" dari sistem AGENT. Tidak ada user yang langsung masuk ke proses pembuatan prompt tanpa melewati layer ini.

**Fungsi utama:**
1. Menanamkan karakter konsisten **Yui** di semua node
2. Menggali kebutuhan user secara adaptif (Discovery Stage)
3. Menganalisis dan menentukan kategori prompt (Classifier)
4. Memberikan kondisi routing ke cabang yang tepat

**Semua node di layer ini menggunakan:** Gemini 3 Flash (default) atau Gemini 3.1 Pro

---

## 📁 Peta File di `01_core`

Di Opal, ada dua cara memasukkan konten ke sebuah node: sebagai **Asset** (referensi eksternal) atau langsung ditulis di dalam **instruksi Generate node**. Berikut pemetaan setiap file:

| File | Cara digunakan di Opal | Fungsi |
|------|----------------------|--------|
| `agent_persona.md` | **Upload sebagai Asset** → dipanggil dengan `@agent_persona` | Berisi karakter Yui — kepribadian, gaya bicara, aturan interaksi |
| `classifier_rules.md` | **Upload sebagai Asset** → dipanggil dengan `@classifier_rules` | Peta kata kunci dan aturan resolusi ambiguitas untuk klasifikasi |
| `quality_gates.md` | **Upload sebagai Asset** → dipanggil dengan `@quality_gates` | Checklist validasi sebelum prompt diberikan ke user |
| `discovery_stage.md` | **Salin instruksinya** → tempel ke dalam Generate Node #1 | System prompt untuk node Discovery (Stage 0) |
| `system_prompt.md` | **Salin instruksinya** → tempel ke dalam Generate Node #2 | System prompt untuk node Classifier (Stage 1) |

---

## ⚙️ Interface Opal — 4 Menu Utama

Sebelum memulai, pahami dulu 4 menu yang tersedia di builder Opal:

```
┌─────────────────────────────────────────────────────┐
│  CANVAS OPAL — 4 Elemen yang Bisa Kamu Tambahkan    │
│                                                     │
│  1. USER INPUT  → Titik masuk data dari pengguna    │
│  2. GENERATE    → Node AI (berisi system prompt)    │
│  3. OUTPUT      → Tampilan hasil akhir ke user      │
│  4. ADD ASSET   → Upload file referensi (.md/.pdf)  │
│                                                     │
│  Koneksi antar-node: panah dengan kondisi routing   │
└─────────────────────────────────────────────────────┘
```

**Penting — Cara Routing/Percabangan di Opal:**
Di Opal, tidak ada "Router Node" tersendiri. Percabangan dilakukan dengan cara:
1. Sebuah Generate node menghasilkan output teks dengan format tertentu (misal `KEPUTUSAN: gambar`)
2. Kamu menghubungkan Generate tersebut ke beberapa node tujuan menggunakan **beberapa panah**
3. Pada setiap panah, kamu set **kondisi** dalam bahasa natural (misal: "jika output mengandung 'KEPUTUSAN: gambar'")
4. Opal mengevaluasi kondisi tersebut dan meneruskan ke panah yang cocok

Selain itu, routing juga bisa dikendalikan secara programatik dari dalam system prompt menggunakan `@tools.goto`.

---

## 📌 Panduan Instalasi Step-by-Step

### TAHAP A — Upload Asset (Lakukan Ini Pertama Kali)

Asset adalah "pengetahuan statis" yang bisa dipanggil oleh node manapun di canvas.

**Langkah:**
1. Buka proyek Opal kamu
2. Klik tombol **"Add Asset"** di canvas
3. Upload file-file berikut **satu per satu**:
   - `01_core/agent_persona.md`
   - `01_core/classifier_rules.md`
   - `01_core/quality_gates.md`
4. Setelah upload, setiap file akan memiliki nama yang bisa dipanggil menggunakan `@` di dalam system prompt Generate node manapun

> **Nama pemanggil di `@` mengikuti nama file** (tanpa ekstensi `.md`).
> Contoh: File `agent_persona.md` → dipanggil dengan `@agent_persona`

---

### TAHAP B — Node 1: User Input

Node ini adalah tampilan awal yang dilihat pengguna.

**Langkah:**
1. Klik **"User Input"** → seret ke canvas
2. Konfigurasi panel di sebelah kanan:
   - **Label:** `Halo! Aku Yui. Kamu mau bikin apa hari ini?`
   - **Placeholder:** `Ceritain aja bebas — mau bikin gambar, musik, kode, atau apapun!`
   - **Tipe Input:** `Text Area`
   - **Required:** ✅ Aktifkan
3. **Nama node ini di canvas:** Ganti jadi `Input_UserGoal`
   *(Nama penting karena dipakai untuk referensi `@Input_UserGoal` di node berikutnya)*

---

### TAHAP C — Node 2: Generate — Discovery Stage

Node ini adalah Yui dalam aksi — menggali info dari user sebelum apapun diproses.

**Langkah:**
1. Klik **"Generate"** → seret ke canvas di bawah/setelah node *Input_UserGoal*
2. Hubungkan panah dari `Input_UserGoal` ke node Generate ini
3. **Ganti nama node** menjadi: `Gen_Discovery`
4. Klik node → buka panel konfigurasi di kanan:

   **A. Pilih Model:**
   - Pilih **Gemini 3 Flash** (cepat, cocok untuk interaksi)

   **B. Mode:**
   - Pilih **Agent Mode** (bukan Fixed)
   - Ini mengaktifkan kemampuan Interactive Chat — Yui bisa tanya-jawab bolak-balik dengan user

   **C. System Instructions (kolom teks utama):**
   - Buka file `discovery_stage.md` dari folder `01_core`
   - Salin semua teks yang ada di dalam blok `---DISCOVERY STAGE PROMPT MULAI---` hingga `---DISCOVERY STAGE PROMPT SELESAI---`
   - Tempel ke kolom **System Instructions** di node ini

   **D. Sambungkan `@` di dalam instruksi:**
   - Di dalam teks yang sudah kamu tempel, temukan baris `@agent_persona`
   - Ketik ulang `@` → pilih `agent_persona` dari dropdown yang muncul
   - ✅ Jika berhasil, teks `@agent_persona` akan berubah menjadi link/biru

   **E. Aktifkan Tools berikut di node ini:**
   - `@tools.memory` → untuk membaca preferensi user dari sesi sebelumnya
   *(Caranya: di kolom instruksi, tulis atau pastikan ada baris yang menyebut `@tools.memory` untuk baca data awal)*

5. **Konfigurasi Interactive Chat:**
   - Di panel node, temukan toggle atau opsi **"Interactive Chat"** → Aktifkan
   - Ini memungkinkan Yui mengirim pesan balik ke user dan menunggu jawaban sebelum lanjut

---

### TAHAP D — Node 3: Generate — Classifier

Node ini membaca hasil Discovery dan mengeluarkan keputusan kategori + sinyal routing.

**Langkah:**
1. Klik **"Generate"** baru → seret ke canvas setelah `Gen_Discovery`
2. **Nama node:** `Gen_Classifier`
3. Hubungkan panah dari `Gen_Discovery` ke `Gen_Classifier`

   > **Kondisi pada panah ini:**
   > Di panel panah (klik panah), set kondisi:
   > `"Jika output mengandung DISCOVERY_COMPLETE: true"`
   > Ini memastikan Classifier hanya jalan setelah Discovery selesai.

4. Konfigurasi panel `Gen_Classifier`:

   **A. Model:** Gemini 3 Flash

   **B. Mode:** Agent Mode

   **C. System Instructions:**
   - Buka file `system_prompt.md` dari folder `01_core`
   - Salin isi instruksinya → tempel ke kolom System Instructions
   - Sambungkan tag-tag berikut di dalam instruksi:
     - `@agent_persona` → karakter Yui tetap aktif
     - `@classifier_rules` → panduan kata kunci klasifikasi
     - `@Gen_Discovery` → baca output dari node Discovery sebelumnya
     - `@tools.memory` → baca preferensi user jika ada dari sesi sebelumnya

   **D. Format Output yang Harus Dihasilkan Classifier:**
   Pastikan di dalam instruksi, Classifier diperintahkan menghasilkan output dalam format ini:
   ```
   KEPUTUSAN: [gambar/video/audio/coding/persona/konten]
   ALASAN: [satu kalimat]
   CATATAN: [info untuk sub-agent cabang]
   ```

---

### TAHAP E — Membuat Percabangan (Routing)

Dari `Gen_Classifier`, tarik **6 panah berbeda** ke 6 node Generate yang berbeda (sub-agent per cabang). Masing-masing panah punya kondisi sendiri.

**Cara membuat kondisi pada panah:**
1. Klik panah yang menghubungkan `Gen_Classifier` ke node tujuan
2. Di panel panah, tambahkan kondisi dalam bahasa natural

**Kondisi untuk setiap jalur:**

| Panah ke | Kondisi yang Ditulis |
|----------|---------------------|
| `Gen_SubAgent_Gambar` | `Output mengandung "KEPUTUSAN: gambar"` |
| `Gen_SubAgent_Video` | `Output mengandung "KEPUTUSAN: video"` |
| `Gen_SubAgent_Audio` | `Output mengandung "KEPUTUSAN: audio"` |
| `Gen_SubAgent_Coding` | `Output mengandung "KEPUTUSAN: coding"` |
| `Gen_SubAgent_Persona` | `Output mengandung "KEPUTUSAN: persona"` |
| `Gen_SubAgent_Konten` | `Output mengandung "KEPUTUSAN: konten"` |
| `Gen_Discovery` *(loop fallback)* | `Tidak ada kondisi di atas yang terpenuhi` |

> **Catatan Fallback:** Kondisi terakhir (loop balik ke Discovery) memastikan jika Classifier menghasilkan output yang tidak dikenali, user diminta klarifikasi ulang — bukan error.

---

### TAHAP F — Node Output Sementara (untuk Testing)

Sebelum sub-agent setiap cabang dibuat, pasang dulu node Output sementara agar bisa testing.

1. Klik **"Output"** → seret ke canvas
2. Hubungkan sementara ke `Gen_Classifier` (tanpa kondisi) untuk melihat output mentahnya
3. **Tipe output:** Text
4. Ini hanya untuk fase testing — nanti akan dihapus dan digantikan oleh Output Final permanen

---

## 🔧 Cheat Sheet: Tag `@` yang Digunakan di Layer `01_core`

| Tag | Dipanggil Di | Fungsi |
|-----|-------------|--------|
| `@agent_persona` | Gen_Discovery, Gen_Classifier | Load karakter Yui |
| `@classifier_rules` | Gen_Classifier | Load panduan kata kunci |
| `@quality_gates` | Gen_FinalPrompt (di cabang) | Load checklist validasi |
| `@Input_UserGoal` | Gen_Discovery | Baca input awal user |
| `@Gen_Discovery` | Gen_Classifier | Baca output hasil Discovery |
| `@tools.memory` | Gen_Discovery, Output_Final | Baca/simpan preferensi lintas sesi |
| `@tools.goto` | (opsional) Gen_Classifier | Routing programatik alternatif |

---

## 🔄 Diagram Canvas Lengkap Layer `01_core`

```
┌─────────────────┐
│  Input_UserGoal │  ← User ketik bebas
└────────┬────────┘
         │ (panah tanpa kondisi)
         ▼
┌─────────────────────────────────────────┐
│  Gen_Discovery                          │
│  Model: Gemini 3 Flash (Agent Mode)     │
│  ✅ Interactive Chat ON                  │
│  → Load: @agent_persona                 │
│  → Baca: @tools.memory                  │
│  → Baca: @Input_UserGoal                │
│  → Output: DISCOVERY_COMPLETE: true +   │
│            data handoff                 │
└────────────────┬────────────────────────┘
                 │ (kondisi: output mengandung DISCOVERY_COMPLETE: true)
                 ▼
┌─────────────────────────────────────────┐
│  Gen_Classifier                         │
│  Model: Gemini 3 Flash (Agent Mode)     │
│  → Load: @agent_persona                 │
│  → Load: @classifier_rules              │
│  → Baca: @Gen_Discovery                 │
│  → Baca: @tools.memory                  │
│  → Output: KEPUTUSAN: [kategori]        │
└──┬──────┬──────┬──────┬──────┬──────┬──┘
   │      │      │      │      │      │
   ▼      ▼      ▼      ▼      ▼      ▼
[IMG]  [VID]  [AUD]  [CODE] [PERSONA][KONTEN]
Sub-Agent per cabang (didefinisikan di 02_branches/)

   ▲ (loop fallback jika tidak ada kondisi cocok)
```

---

## 🧪 Prosedur Testing Layer `01_core`

Jalankan mode **Preview** di Opal dan uji 3 skenario berikut sebelum membangun cabang:

### Test 1: Fast Track
**Input:** `"Yui, buatin prompt gambar kucing astronot style watercolor pastel buat Midjourney"`

**Ekspektasi:**
- ✅ Yui tidak bertanya apapun
- ✅ Langsung memuji inputnya (*"Wah, super spesifik!"*)
- ✅ `Gen_Classifier` menghasilkan `KEPUTUSAN: gambar`
- ✅ Panah menuju `Gen_SubAgent_Gambar` aktif

### Test 2: Full Discovery
**Input:** `"Bikinin konten."`

**Ekspektasi:**
- ✅ Yui bertanya tentang platform (maks 3 pertanyaan bergiliran)
- ✅ Setiap pertanyaan menyertakan alasan kenapa perlu ditanya
- ✅ Setelah 3 pertanyaan, agent membuat best-guess dan lanjut
- ✅ `DISCOVERY_COMPLETE: true` muncul di output

### Test 3: Persona Check
**Input:** `"Hello, siapa kamu?"`

**Ekspektasi:**
- ✅ Yui memperkenalkan diri dengan gaya santai, bukan formal
- ✅ Tidak ada kalimat `"Sebagai AI..."`
- ✅ Menggunakan kata "aku" dan "kamu"

**Jika Test 3 gagal:** Periksa apakah `@agent_persona` di node `Gen_Discovery` sudah menjadi link biru (terhubung ke asset yang diupload). Jika masih teks biasa, hapus dan ketik ulang `@agent_persona` → pilih dari dropdown.

---

## 📝 Checklist Sebelum Lanjut ke Cabang (02_branches)

```
□ Assets sudah diupload: agent_persona.md, classifier_rules.md, quality_gates.md
□ Input_UserGoal sudah ada dan required = true
□ Gen_Discovery: mode Agent, Interactive Chat ON, @agent_persona terhubung
□ Gen_Discovery: @tools.memory aktif untuk baca preferensi
□ Gen_Classifier: @agent_persona, @classifier_rules, @Gen_Discovery terhubung
□ 6 panah percabangan + 1 panah fallback dari Gen_Classifier sudah ada
□ Kondisi routing di setiap panah sudah sesuai tabel di atas
□ Test 1, 2, dan 3 sudah berhasil
□ Semua node diberi nama sesuai konvensi (Input_*, Gen_*)
```

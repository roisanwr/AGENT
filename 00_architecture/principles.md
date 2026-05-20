# Prinsip Desain — AGENT

> Dokumen ini menjelaskan **mengapa** sistem ini dirancang dengan cara tertentu. Setiap keputusan desain ada alasannya.

---

## Prinsip 1: Modular by Default

**Apa artinya:**
Setiap cabang adalah unit yang independen. Menambah cabang baru tidak memerlukan perubahan pada cabang yang sudah ada.

**Mengapa penting:**
Sistem yang monolitik (satu agent untuk semua) cepat menjadi kompleks dan sulit di-maintain. Ketika ada masalah di cabang "video", tidak boleh mempengaruhi cabang "coding".

**Implementasinya:**
- Setiap cabang punya file rules sendiri di `02_branches/`
- Sub-agent per cabang hanya tahu tentang domain-nya sendiri
- Dynamic router adalah satu-satunya komponen yang "tahu" semua cabang

---

## Prinsip 2: Specificity Beats Creativity

**Apa artinya:**
Model AI sudah sangat kreatif. Tugas kita bukan menginspirasi model — tapi **membatasi ruang eksplorasi** ke arah yang tepat.

**Mengapa penting:**
Prompt yang terlalu umum → output yang random. Prompt yang spesifik → output yang predictable dan sesuai harapan. Setiap fixed step di setiap cabang dirancang untuk **mengeliminasi ambiguitas** satu per satu.

**Implementasinya:**
- Fixed steps per cabang menggali parameter spesifik
- Setiap parameter yang dikumpulkan mengurangi "ruang interpretasi" model
- Quality gates memastikan tidak ada parameter kritis yang kosong sebelum generate

---

## Prinsip 3: Knowledge Separated from Logic

**Apa artinya:**
Rules tentang cara kerja sistem (logic) dipisah dari pengetahuan tentang model AI (knowledge).

**Mengapa penting:**
Model AI terus update fitur dan sintaksnya. Jika knowledge dijadikan satu dengan logic workflow di Opal, setiap kali Midjourney rilis fitur baru, kamu harus buka dan edit workflow — risiko merusak yang lain sangat tinggi. Dengan memisahkannya ke file Markdown, update knowledge = edit file, selesai.

**Implementasinya:**
- Logic ada di Opal (agent step, routing, fixed steps)
- Knowledge ada di `03_models/` (Markdown files)
- Agent "membaca" knowledge saat dibutuhkan via memory step

---

## Prinsip 4: Human-in-the-Loop di Titik Kritis

**Apa artinya:**
Agent tidak autorun sampai selesai tanpa konfirmasi manusia. Ada checkpoint strategis di mana user perlu memberikan input atau approval.

**Checkpoint yang didefinisikan:**
1. **Klarifikasi input** — jika input ambigu
2. **Konfirmasi model** — sebelum generate, user pilih/konfirmasi target model
3. **Preview prompt** — sebelum output final, user review dan approve

**Mengapa penting:**
Prompt engineering adalah proses yang sangat subjektif. Tanpa checkpoint, sistem mungkin generate prompt yang "secara teknis benar" tapi tidak sesuai dengan visi user. Loop feedback adalah yang membedakan output biasa dengan output yang benar-benar bagus.

---

## Prinsip 5: One Question at a Time

**Apa artinya:**
Sub-agent tidak boleh mengajukan beberapa pertanyaan sekaligus. Satu step = satu parameter = satu pertanyaan.

**Mengapa penting:**
User yang dihadapkan dengan banyak pertanyaan sekaligus cenderung memberikan jawaban yang kurang detail atau malah skip. Dengan satu pertanyaan per step, user lebih fokus dan jawaban yang didapat lebih berkualitas.

**Implementasinya:**
- Setiap fixed step hanya menggali satu parameter
- Pertanyaan harus disertai contoh/pilihan untuk memudahkan user
- Step berikutnya di-unlock setelah step sebelumnya selesai

---

## Prinsip 6: Default Over Blocking

**Apa artinya:**
Jika user tidak menjawab parameter opsional, gunakan default yang masuk akal — jangan stop workflow.

**Mengapa penting:**
Tidak semua user tahu detail teknis seperti aspect ratio atau CFG scale. Jika sistem "blocking" karena ada parameter yang tidak diisi, pengalaman user menjadi buruk. Default yang cerdas lebih baik daripada workflow yang terhenti.

**Aturan untuk default:**
- Default harus dipilih berdasarkan "yang paling sering diinginkan user" untuk kategori itu
- Default harus disebutkan ke user: "Saya pakai aspect ratio 16:9 sebagai default"
- Default bisa di-override kapan saja

---

## Prinsip 7: Naming Convention yang Konsisten

**Apa artinya:**
Semua step, variabel, dan file mengikuti konvensi penamaan yang sudah ditetapkan.

**Konvensi yang berlaku:**

| Tipe | Format | Contoh |
|------|--------|--------|
| File cabang | `[nama].md` | `image.md`, `video.md` |
| File model | `[nama_model].md` | `midjourney.md`, `suno.md` |
| Step di Opal | `[kategori]_[fungsi]` | `image_subject`, `coding_language` |
| Variabel output | `@[nama_step]` | `@image_subject`, `@model_selected` |

**Mengapa penting:**
Konsistensi penamaan membuat sistem jauh lebih mudah di-debug dan diperluas. Ketika kamu menambah cabang ke-10, kamu sudah tahu persis bagaimana menamainya.

---

## Prinsip 8: Fail-Safe, Bukan Fail-Fast

**Apa artinya:**
Ketika ada error atau input yang tidak diharapkan, sistem harus "jatuh dengan aman" — bukan crash atau menghasilkan output yang salah.

**Implementasinya:**
- Classifier selalu punya kondisi `fallback` untuk kategori yang tidak dikenali
- Generate step melakukan validasi sebelum output
- Jika routing salah, ada mekanisme untuk kembali ke classifier

**Error yang harus ditangani:**
1. Input yang tidak bisa diklasifikasi → tanya user, jangan asumsikan
2. Parameter yang kurang → pakai default, beri tahu user
3. Model tidak tersedia → rekomendasikan alternatif
4. Output yang terlalu panjang → potong dan beri tahu user

---

## Anti-Pattern yang Harus Dihindari

### ❌ Monolithic Agent
Satu agent yang mencoba melakukan segalanya dari classifikasi sampai generate. Sulit di-debug dan tidak scalable.

### ❌ Knowledge di System Prompt
Menjejalkan semua knowledge model AI ke dalam system prompt classifier. Membuatnya panjang, sulit di-update, dan rentan overwrite konteks.

### ❌ Routing Tanpa Fallback
Dynamic routing tanpa kondisi fallback. Jika ada input yang tidak cocok, workflow akan error.

### ❌ Generate Tanpa Validasi
Langsung output ke user tanpa quality gate dan preview. Menghasilkan prompt yang tidak konsisten kualitasnya.

### ❌ Parameter Bundling
Mengajukan beberapa pertanyaan sekaligus dalam satu step. Mengurangi kualitas jawaban dan pengalaman user.

---

## Evolusi Arsitektur

Dokumen ini mencatat bagaimana arsitektur berkembang seiring waktu:

| Versi | Perubahan Utama | Alasan |
|-------|----------------|--------|
| v1.0 | Arsitektur dasar: 6 cabang, 8 step | Fondasi awal |
| v1.1 | Tambah Model Selector layer | Model AI target sangat mempengaruhi kualitas prompt |
| v1.2 | Knowledge base dipisah ke `03_models/` | Kemudahan update tanpa edit Opal |
| *v2.0* | *[Planned]* Multi-user support | - |

---

*Dokumen ini harus diperbarui setiap kali ada keputusan arsitektur baru*

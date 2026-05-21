# Cabang GAMBAR — Rules & Fixed Steps

| Field | Value |
|-------|-------|
| **Nama Cabang** | `image` |
| **Label** | Gambar / Image / Ilustrasi |
| **Target Model** | Midjourney, DALL-E 3, Stable Diffusion, Flux, Ideogram |
| **Versi** | 1.0 |

---

## 🛡️ PRE-KONDISI WAJIB — Baca Sebelum Mulai [FIX Celah #4]

SEBELUM mengajukan pertanyaan apapun ke user, Sub-Agent WAJIB:

1. **Baca variabel berikut dari output Classifier (yang diteruskan dari Discovery):**

   | Variabel | Status | Aksi |
   |----------|--------|-----------|
   | `SUBJEK` | ✅ Terisi → Tandai SELESAI, JANGAN tanya ulang | ❌ Kosong → Tanya di Step 1 |
   | `PLATFORM_TARGET` | ✅ Terisi → Gunakan untuk rekomendasikan model | ❌ Kosong → Tanya saat Model Selector |
   | `EKSPEKTASI` | ✅ Terisi → Petakan ke `@image_style` dan `@image_mood` | ❌ Kosong → Tanya di Step 2/3 |
   | `MODEL_PREFERENSI` | ✅ Terisi → Skip model selection | ❌ Kosong → Rekomendasikan saat Model Selector |

2. **DILARANG bertanya ulang variabel yang sudah ✅ SELESAI dari Discovery.**

3. **Hanya tanyakan parameter yang MASIH KOSONG.**

4. **Jika semua parameter sudah terisi dari Discovery**, langsung konfirmasi ringkasan dan proceed ke generate.

---

## 📊 Nilai Default Wajib (Hardcoded) [FIX Celah #3]

> Default ini digunakan ketika parameter tidak dijawab user dan tidak ada dari Discovery.
> Quality Gate akan membaca tabel ini — BUKAN menebak sendiri.

| Parameter | Default Jika Kosong | Kapan Dipakai |
|-----------|---------------------|---------------|
| `@image_style` | `digital illustration` | Jika Step 2 dilewati / tidak dijawab |
| `@image_mood` | `soft diffused light, neutral` | Jika Step 3 dilewati / tidak dijawab |
| `@image_ratio` | `1:1` | Jika Step 4 dilewati / tidak dijawab |
| `@image_details` | *(kosong — tidak perlu default)* | Step 5 opsional, boleh tidak diisi |
| quality suffix MJ | `--quality 1 --style raw` | Default Midjourney jika tidak ada preferensi |
| negative prompt SD | `blurry, low quality, watermark, bad anatomy, extra limbs, deformed` | Default SD |

---


**Kuat:**
```
gambar, image, foto, ilustrasi, artwork, art, lukisan, drawing, sketch,
poster, thumbnail, logo, banner, background, wallpaper, portrait,
concept art, character design, render, ilustrasi
```

**Pendukung:**
```
visual, warna, style, aesthetic, realistis, anime, cartoon, 3D,
midjourney, dall-e, stable diffusion, flux, ideogram, pixel art
```

**Ambigu:**
- "konten visual" → tanya: "Gambar diam atau video bergerak?"
- "thumbnail video" → **GAMBAR** (output statis)
- "cover album" → tanya: "Artwork/gambar untuk cover, atau musik albumnya?"

---

## Fixed Steps — Parameter yang Digali

### Step 1 — Subjek Utama
- **Cek Discovery dulu:** Jika `SUBJEK` sudah ada dari Discovery → lewati pertanyaan ini, tandai ✅ SELESAI.
- **Pertanyaan (jika SUBJEK kosong):** *"Siapa atau apa yang jadi fokus utama gambarmu? Semakin spesifik semakin bagus."*
- **Contoh yang diberikan:** "seorang warrior wanita berambut merah", "naga emas terbang di awan", "kota futuristik malam hari"
- **Default:** tidak ada — ini parameter WAJIB, tanya sampai ada
- **Variabel:** `@image_subject`

### Step 2 — Style & Medium
- **Pertanyaan:** *"Style visual apa yang kamu inginkan?"*
- **Pilihan:**
  - `oil painting / lukisan cat minyak`
  - `digital illustration`
  - `cinematic photography / foto sinematik`
  - `anime / manga style`
  - `flat vector illustration`
  - `3D render / CGI`
  - `watercolor`
  - `pixel art`
  - `sketch / lineart`
  - *(atau ketik sendiri)*
- **Default:** `digital illustration`
- **Variabel:** `@image_style`

### Step 3 — Mood & Lighting
- **Pertanyaan:** *"Bagaimana mood dan pencahayaannya? Ini sangat mempengaruhi feel gambar."*
- **Pilihan:**
  - `golden hour — hangat dan dramatis`
  - `dramatic side lighting — kontras tinggi`
  - `soft diffused light — lembut dan dreamy`
  - `dark and moody — gelap, atmosferik`
  - `bright and vibrant — cerah dan energik`
  - `neon/cyberpunk glow`
  - *(atau deskripsikan sendiri)*
- **Default:** `soft diffused light`
- **Variabel:** `@image_mood`

### Step 4 — Komposisi & Aspect Ratio
- **Pertanyaan:** *"Ukuran/orientasi gambarnya?"*
- **Pilihan:**
  - `1:1 — Square (postingan Instagram, icon)`
  - `16:9 — Landscape (thumbnail YouTube, wallpaper)`
  - `9:16 — Portrait (story, Reels, TikTok)`
  - `4:3 — Classic landscape`
  - `3:2 — Foto DSLR standard`
  - `2:3 — Portrait foto`
- **Default:** `1:1`
- **Variabel:** `@image_ratio`

### Step 5 — Detail Tambahan (Opsional)
- **Pertanyaan:** *"Ada detail lain yang penting? (misal: warna dominan, referensi artis, elemen yang TIDAK boleh ada)"*
- **Default:** kosong (skip jika tidak dijawab)
- **Variabel:** `@image_details`

---

## Struktur Prompt Output

```
[subjek_detail] + [setting/latar] + [style/medium] + [lighting/mood] + [atmosfer] + [quality_boosters] [parameter_model]
```

**Template untuk Midjourney:**
```
{@image_subject}, {latar jika ada}, {@image_style}, {@image_mood}, highly detailed, award-winning, 8K --ar {ratio} --style raw --no watermark, blur, extra limbs
```

**Template untuk DALL-E 3:**
```
{Kalimat deskriptif natural tentang @image_subject}. The scene is set {latar} with {style} style. Lighting: {@image_mood}. Include {detail_tambahan}. Professional quality, highly detailed.
```

**Template untuk Stable Diffusion:**
```
[Positive]: {@image_subject}, {@image_style}, {@image_mood}, masterpiece, best quality, highly detailed
[Negative]: blurry, low quality, watermark, bad anatomy, extra limbs, deformed
[Ratio]: {@image_ratio}
```

**Template untuk Flux:**
```
{Deskripsi subjek dengan fokus pada material dan tekstur}. {@image_style} style. {@image_mood} atmosphere. Ultra-detailed {material} texture. {@image_ratio} aspect ratio.
```

---

## Quality Checklist

**WAJIB sebelum generate:**
```
□ Subjek utama sudah spesifik (bukan generik seperti "wanita cantik")
□ Style/medium sudah ditentukan
□ Aspect ratio sudah dipilih
□ Target model sudah dipilih
```

**SANGAT DIREKOMENDASIKAN:**
```
□ Lighting sudah disebutkan (lighting = 50% mood gambar)
□ Format sesuai model target
□ Quality boosters ditambahkan di akhir
□ Negative prompt ada (untuk MJ dan SD)
```

---

## Jebakan Umum & Solusinya

**Jebakan 1: Subjek terlalu generik**
> "Wanita cantik" atau "pemandangan indah" → model pilih interpretasi random
> **Solusi:** Arahkan user untuk mendeskripsikan detail spesifik: usia, rambut, ekspresi, pakaian, dll.

**Jebakan 2: Style yang kontradiksi**
> "Realistic anime photography" → model bingung
> **Solusi:** Pilih SATU genre visual utama. Gabungan hanya dengan "inspired by" bukan "is a"

**Jebakan 3: Terlalu banyak subjek**
> Lebih dari 2-3 elemen utama → satu elemen akan diabaikan
> **Solusi:** Tentukan satu "hero element" yang paling penting

**Jebakan 4: Tidak ada aspect ratio**
> Komposisi berbeda total antara portrait dan landscape
> **Solusi:** Selalu tanyakan di Step 4 — tidak boleh di-skip

---

## System Prompt Sub-Agent

```
---SYSTEM PROMPT SUB-AGENT GAMBAR---

Kamu adalah specialist image prompt engineer. Tugasmu: membantu user mendefinisikan semua elemen yang dibutuhkan untuk menghasilkan gambar AI yang luar biasa.

⛔ PRE-KONDISI WAJIB — Jalankan ini SEBELUM bertanya apapun:
1. Baca variabel dari Classifier: SUBJEK, PLATFORM_TARGET, EKSPEKTASI, MODEL_PREFERENSI
2. Tandai variabel yang sudah terisi sebagai ✅ SELESAI
3. JANGAN tanya ulang variabel yang sudah ✅ SELESAI
4. Mulai dari Step yang parameter-nya masih kosong

Kamu akan menggali 4-5 parameter secara berurutan, satu per satu:
1. Subjek utama (WAJIB — paling detail mungkin) → lewati jika SUBJEK sudah ada dari Discovery
2. Style dan medium visual → lewati jika ada di EKSPEKTASI
3. Mood dan pencahayaan → lewati jika ada di EKSPEKTASI
4. Aspect ratio / komposisi
5. Detail tambahan (opsional)

ATURAN:
- Tanya satu parameter per giliran
- Selalu berikan pilihan contoh untuk memudahkan
- Subjek adalah yang paling kritis — dorong user untuk spesifik
- Jika user tidak menjawab step 3-5, gunakan DEFAULT dari tabel NILAI DEFAULT WAJIB
- JANGAN menebak nilai default di luar tabel tersebut

INGAT: Kamu mengumpulkan bahan, bukan generate prompt. Setelah semua terkumpul, serahkan ke generate step.

---SYSTEM PROMPT SELESAI---
```

---

## Knowledge Model

Lihat detail knowledge per model di:
- [`../03_models/image/midjourney.md`](../03_models/image/midjourney.md)
- [`../03_models/image/dalle3.md`](../03_models/image/dalle3.md)
- [`../03_models/image/stable_diffusion.md`](../03_models/image/stable_diffusion.md)
- [`../03_models/image/flux.md`](../03_models/image/flux.md)
- [`../03_models/image/ideogram.md`](../03_models/image/ideogram.md)

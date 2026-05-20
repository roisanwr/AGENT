# Cabang VIDEO — Rules & Fixed Steps

| Field | Value |
|-------|-------|
| **Nama Cabang** | `video` |
| **Label** | Video / Film / Animasi |
| **Target Model** | Google Veo 2/3, Sora, Kling, Runway Gen, Pika |
| **Versi** | 1.0 |

---

## Konsep Fundamental

> **Prompt video berbeda fundamental dengan prompt gambar.**
> 
> Gambar: deskripsi visual statis.  
> Video: deskripsi **gerak, waktu, dan narasi temporal**.
>
> Kesalahan terbesar: menggunakan "gaya gambar" untuk video → hasil flat dan tanpa dinamika.

---

## Fixed Steps — Parameter yang Digali

### Step 1 — Camera Movement & Shot Type ⚠️ KRITIS
- **Pertanyaan:** *"Bagaimana kamera bergerak? Ini yang paling sering dilupakan dan paling berpengaruh."*
- **Pilihan:**
  - `Static wide shot — kamera diam, angle lebar`
  - `Slow dolly in — kamera maju perlahan ke subjek`
  - `Aerial drone shot pulling back — tampak dari atas, mundur`
  - `Handheld tracking shot — ikuti gerakan subjek`
  - `360 orbit around subject — melingkar di sekitar subjek`
  - `POV / first person — sudut pandang orang pertama`
  - `Cinematic pan — kamera geser horizontal`
  - *(atau deskripsikan sendiri)*
- **Default:** `static wide shot`
- **Variabel:** `@video_camera`

### Step 2 — Subjek & Aksi (Sequential)
- **Pertanyaan:** *"Apa yang terjadi di video ini? Ceritakan dari awal sampai akhir klip."*
- **Panduan:** Bantu user berpikir secara temporal: "Di awal... lalu... dan di akhir..."
- **Contoh:** "Seorang pria berjalan perlahan menuju ombak, angin menggerakkan rambutnya, ia berhenti dan menatap laut"
- **Default:** tidak ada — ini parameter WAJIB
- **Variabel:** `@video_action`

### Step 3 — Durasi & Pace
- **Pertanyaan:** *"Berapa lama videonya dan bagaimana kecepatannya?"*
- **Pilihan:**
  - `5 detik, slow motion — setiap detail terlihat jelas`
  - `5 detik, normal pace — natural`
  - `10 detik, slow motion`
  - `10 detik, normal pace`
  - `30 detik, montage — quick cuts antara beberapa shot`
- **Default:** `5 detik, normal pace`
- **Variabel:** `@video_duration`

### Step 4 — Tone Visual
- **Pertanyaan:** *"Suasana visual yang diinginkan?"*
- **Pilihan:**
  - `Cinematic, warm tones — seperti film bioskop`
  - `Dark noir, high contrast — dramatis dan misterius`
  - `Bright and cheerful — cerah, energik`
  - `Dreamy and soft — lembut, ethereal`
  - `Gritty and raw — realistis, kasar`
  - `Neon cyberpunk — futuristik berwarna-warni`
- **Default:** `cinematic, warm tones`
- **Variabel:** `@video_tone`

### Step 5 — Audio (Opsional)
- **Pertanyaan:** *"Apakah kamu ingin generate audio juga bersamaan? (beberapa model bisa)"*
- **Pilihan:** `Ya, include ambient sound` / `Ya, include music` / `Tidak, video saja`
- **Default:** `Tidak`
- **Variabel:** `@video_audio`

---

## Struktur Prompt Output

```
[camera_movement + shot_type] + [subjek + aksi_sequential] + [setting] + [tone_visual] + [durasi] + [kualitas]
```

**Template untuk Veo 2/3:**
```
{@video_camera}, {@video_action}. Setting: {latar jika ada}. {@video_tone} visual style. {durasi} clip. Photorealistic, 4K cinema quality. {audio jika ya: include natural ambient sound}
```

**Template untuk Sora:**
```
{@video_camera}. {@video_action}. The environment is {setting}. {@video_tone} atmosphere. Physics-accurate movement. {durasi} continuous shot.
```

**Template untuk Kling:**
```
{@video_camera}, {durasi} video. {@video_action}. {@video_tone}. Smooth motion, high quality render.
```

---

## Quality Checklist

**WAJIB sebelum generate:**
```
□ Camera movement sudah disebutkan (wajib — bukan opsional!)
□ Aksi mendeskripsikan GERAK dan WAKTU (bukan deskripsi statis)
□ Durasi sudah ditentukan
□ Target model sudah dipilih
```

**SANGAT DIREKOMENDASIKAN:**
```
□ Tone visual sudah ada
□ Setting/latar sudah disebutkan
□ Satu prompt = satu continuous shot (bukan multi-scene)
```

---

## Jebakan Umum & Solusinya

**Jebakan 1: Prompt statis seperti gambar**
> "Seorang pria berdiri di pantai" → video flat tanpa dinamika
> **Solusi:** Tambahkan aksi temporal: "berjalan perlahan menuju ombak, angin menggerakkan rambutnya, ia berhenti dan menatap laut"

**Jebakan 2: Terlalu banyak scene dalam satu prompt**
> Mencoba ceritakan 3-4 scene berbeda → model bingung atau abaikan sebagian
> **Solusi:** Satu prompt = satu continuous shot. Multi-scene = buat prompt terpisah per scene.

**Jebakan 3: Tidak ada camera movement**
> Ini yang paling sering dilupakan dan paling berpengaruh pada kualitas
> **Solusi:** Step 1 adalah camera movement — ini KRITIS, tidak boleh di-skip

**Jebakan 4: Durasi tidak realistis**
> Meminta narasi panjang dalam 5 detik
> **Solusi:** Sesuaikan kompleksitas aksi dengan durasi yang realistis

---

## System Prompt Sub-Agent

```
---SYSTEM PROMPT SUB-AGENT VIDEO---

Kamu adalah specialist video prompt engineer. Kamu paham bahwa prompt video BERBEDA FUNDAMENTAL dengan prompt gambar.

Prinsip terpenting: Video adalah GERAK + WAKTU, bukan deskripsi visual statis.

Kamu akan menggali parameter ini secara berurutan:
1. Camera movement & shot type (PALING KRITIS — tanya dulu ini)
2. Subjek + aksi secara TEMPORAL (dari awal ke akhir)
3. Durasi & pace
4. Tone visual
5. Audio (opsional)

ATURAN KHUSUS VIDEO:
- Satu prompt = satu continuous shot
- Dorong user untuk mendeskripsikan AKSI secara berurutan waktu
- Selalu ingatkan: "Apa yang terjadi dari awal hingga akhir clip?"
- Jangan terima deskripsi yang statis — minta user tambahkan gerakan

---SYSTEM PROMPT SELESAI---
```

---

## Knowledge Model

- [`../03_models/video/veo.md`](../03_models/video/veo.md)
- [`../03_models/video/sora.md`](../03_models/video/sora.md)
- [`../03_models/video/kling.md`](../03_models/video/kling.md)
- [`../03_models/video/runway.md`](../03_models/video/runway.md)

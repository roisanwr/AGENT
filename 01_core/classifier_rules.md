# Rules Klasifikasi & Routing — AGENT

> Dokumen ini mendefinisikan aturan detil untuk klasifikasi intent dan dynamic routing di Opal.

---

## Peta Kata Kunci per Kategori

Gunakan tabel ini sebagai referensi ketika membangun kondisi di Opal Dynamic Routing.

### 🖼️ GAMBAR

**Kata kunci kuat (konfidenssi tinggi):**
```
gambar, image, foto, foto-realistis, ilustrasi, artwork, art, lukisan, 
drawing, sketch, poster, thumbnail, logo, icon, banner, background, 
wallpaper, portrait, landscape, character design, concept art
```

**Kata kunci pendukung:**
```
visual, warna, style, aesthetic, realistis, anime, cartoon, 3D, render,
midjourney, dall-e, stable diffusion, flux, ideogram
```

**Frasa yang sering muncul:**
```
"buat gambar...", "generate image...", "bikin ilustrasi...", 
"desain logo...", "buat artwork...", "foto dengan style..."
```

---

### 🎬 VIDEO

**Kata kunci kuat:**
```
video, film, animasi, klip, short video, reel, sinematik, motion,
timelapse, slow motion, footage, shot, scene, trailer
```

**Kata kunci pendukung:**
```
kamera, gerakan, transisi, durasi, fps, cinematic, veo, sora, kling,
runway, pika
```

**Frasa yang sering muncul:**
```
"buat video...", "generate footage...", "bikin animasi...",
"video pendek untuk...", "klip sinematik..."
```

---

### 🎵 AUDIO

**Kata kunci kuat:**
```
musik, audio, lagu, song, music, nada, melodi, beat, instrumental,
jingle, soundtrack, sound, suara, sfx, sound effect, voice
```

**Kata kunci pendukung:**
```
genre, bpm, tempo, vokal, lirik, chord, suno, udio, elevenlabs,
musicgen, udio
```

**Frasa yang sering muncul:**
```
"buat musik...", "generate lagu...", "bikin soundtrack...",
"musik untuk video...", "jingle brand..."
```

---

### 💻 CODING

**Kata kunci kuat:**
```
kode, code, program, script, fungsi, function, class, API, debugging,
refactoring, arsitektur, backend, frontend, database, query
```

**Kata kunci pendukung:**
```
python, javascript, php, java, typescript, react, laravel, django,
sql, html, css, github, terminal, CLI
```

**Frasa yang sering muncul:**
```
"buat fungsi...", "coding untuk...", "script yang bisa...",
"debug kode ini...", "refactor...", "implementasikan..."
```

---

### 🤖 PERSONA

**Kata kunci kuat:**
```
persona, karakter, character, chatbot, asisten, assistant, bot,
roleplay, AI character, virtual, kepribadian, personality
```

**Kata kunci pendukung:**
```
system prompt, instruksi karakter, nama, latar belakang, backstory,
cara bicara, tone, batasan karakter
```

**Frasa yang sering muncul:**
```
"buat system prompt untuk...", "karakter AI yang...",
"chatbot dengan kepribadian...", "instruksi untuk asisten...",
"roleplay sebagai..."
```

---

### 📝 KONTEN

**Kata kunci kuat:**
```
artikel, tulisan, caption, copywriting, email, blog, script, narasi,
konten, content, teks, text, copy, headline, hook
```

**Kata kunci pendukung:**
```
Instagram, LinkedIn, YouTube, TikTok, Twitter, website, newsletter,
SEO, marketing, promosi, edukasi, persuasif, informatif
```

**Frasa yang sering muncul:**
```
"tulis artikel...", "buat caption...", "script untuk...",
"copywriting produk...", "email marketing...", "konten untuk..."
```

---

## Matriks Ambiguitas

Beberapa kombinasi kata kunci sering menyebabkan kebingungan. Gunakan ini sebagai panduan:

| Input User | Kemungkinan Cabang | Cara Resolusi |
|-----------|-------------------|---------------|
| "buat konten visual" | GAMBAR atau KONTEN | Tanya: "Konten tulisan atau gambar visual?" |
| "script untuk video" | VIDEO atau KONTEN | Tanya: "Script yang akan dibacakan, atau deskripsi untuk generate video?" |
| "persona chatbot" | PERSONA atau KONTEN | Kepastian tinggi → PERSONA |
| "musik dan lirik" | AUDIO atau KONTEN | Tanya: "Mau generate musiknya, atau cuma tuliskan liriknya?" |
| "kode dan dokumentasi" | CODING → generate docs juga | Routing ke CODING, sub-agent gali lebih lanjut |
| "thumbnail video" | GAMBAR (bukan VIDEO) | Thumbnail adalah output statis → GAMBAR |
| "cover lagu" | AUDIO | Konfirmasi musik, bukan artwork |
| "cover art album" | GAMBAR | Konfirmasi artwork/gambar |

---

## Aturan Routing di Opal

### Cara Mendefinisikan Kondisi di Opal Dynamic Routing

Format kondisi yang direkomendasikan untuk setiap jalur:

**Jalur GAMBAR:**
```
Jika output classifier mengandung "KATEGORI: gambar"
→ Rutekan ke sub-agent Gambar
```

**Jalur VIDEO:**
```
Jika output classifier mengandung "KATEGORI: video"
→ Rutekan ke sub-agent Video
```

**Jalur AUDIO:**
```
Jika output classifier mengandung "KATEGORI: audio"
→ Rutekan ke sub-agent Audio
```

**Jalur CODING:**
```
Jika output classifier mengandung "KATEGORI: coding"
→ Rutekan ke sub-agent Coding
```

**Jalur PERSONA:**
```
Jika output classifier mengandung "KATEGORI: persona"
→ Rutekan ke sub-agent Persona
```

**Jalur KONTEN:**
```
Jika output classifier mengandung "KATEGORI: konten"
→ Rutekan ke sub-agent Konten
```

**Fallback (wajib ada!):**
```
Jika tidak ada kondisi di atas yang terpenuhi
→ Kembali ke classifier dengan pesan: "Kategori tidak teridentifikasi, minta klarifikasi"
```

---

## Prosedur Menambah Cabang Baru

Ketika ada cabang baru yang ingin ditambahkan (contoh: "Presentasi"):

1. Tentukan **kata kunci kuat** untuk kategori baru
2. Identifikasi **potensi ambiguitas** dengan cabang yang sudah ada
3. Tambahkan ke tabel kata kunci di atas
4. Update bagian `## KATEGORI YANG TERSEDIA` di `01_core/system_prompt.md`
5. Tambahkan kondisi routing baru di Opal
6. Buat file `02_branches/[nama_cabang].md`

> Lihat panduan lengkap di [`05_operations/how_to_add_branch.md`](../05_operations/how_to_add_branch.md)

---

## Testing Routing

Kasus uji yang harus berhasil sebelum cabang dianggap production-ready:

| Input | Expected Routing | Status |
|-------|-----------------|--------|
| "buat gambar panda lucu anime style" | GAMBAR | ✅ |
| "video pendek aesthetic untuk Instagram" | VIDEO | ✅ |
| "musik upbeat untuk intro YouTube" | AUDIO | ✅ |
| "fungsi Python untuk sorting list" | CODING | ✅ |
| "system prompt untuk chatbot customer service" | PERSONA | ✅ |
| "artikel blog tentang AI untuk pemula" | KONTEN | ✅ |
| "buat konten" (ambigu) | KLARIFIKASI | ✅ |
| "sesuatu yang keren" (terlalu ambigu) | KLARIFIKASI | ✅ |

---

*Update dokumen ini setiap kali ada perubahan pada logika routing*

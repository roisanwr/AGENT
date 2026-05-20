# Knowledge: DALL-E 3

| Field | Value |
|-------|-------|
| **Model** | DALL-E 3 (OpenAI) |
| **Kategori** | Image Generation |
| **Versi Dokumen** | 1.0 |
| **Terakhir Update** | Mei 2026 |
| **Platform** | ChatGPT, OpenAI API, Bing Image Creator |

---

## Karakteristik Model

**Kekuatan:**
- Pemahaman bahasa natural yang sangat baik — tidak perlu keyword-style
- Teks dalam gambar: terbaik di kelasnya untuk akurasi teks
- Mengikuti instruksi panjang dan detail dengan akurat
- Konsistensi instruksi yang sangat tinggi
- Safety filter yang lebih ketat tapi bisa dinavigasi dengan framing yang tepat

**Kelemahan:**
- Lebih terbatas dalam gaya artistik ekstrem dibanding MJ
- Tidak ada negative prompt (harus framing positif)
- Tidak ada parameter teknis seperti MJ
- Kurang "artistik secara natural" — lebih literal

**Pendekatan Fundamental:**
> DALL-E 3 **BERBEDA** dengan Midjourney. Jangan pakai gaya keyword-stack.  
> DALL-E 3 merespons terbaik pada **kalimat natural deskriptif yang panjang**.

---

## Format Prompt Optimal

**Yang TIDAK bekerja di DALL-E 3:**
```
❌ "warrior woman, silver hair, epic, cinematic, 4K, masterpiece, fantasy"
```

**Yang BEKERJA dengan baik:**
```
✅ "A dramatic oil painting of a warrior woman with silver hair and battle scars. 
She stands on a cliff overlooking a stormy ocean, backlit by a setting sun 
that creates a warm rim light around her silhouette. The style is reminiscent 
of classical fantasy art with rich, saturated colors and dramatic lighting."
```

**Perbedaan kunci:**
- MJ: keyword list pendek → bagus
- DALL-E 3: kalimat deskriptif lengkap → bagus

---

## Struktur Prompt Optimal

```
[Deskripsi pembukaan: apa yang kamu lihat] 
[Deskripsi subjek dalam kalimat natural] 
[Deskripsi setting/latar] 
[Deskripsi lighting dan suasana] 
[Style/medium referensi] 
[Detail tambahan jika ada]
```

**Contoh lengkap:**
```
A photorealistic portrait of an elderly Japanese woman sitting in a traditional 
tea house. She wears a deep blue kimono with subtle cherry blossom patterns. 
Soft morning light filters through shoji screens, casting gentle shadows on 
her weathered but peaceful face. She holds a ceramic tea cup with both hands. 
The atmosphere is serene and contemplative. Shot in the style of professional 
documentary photography with beautiful bokeh in the background.
```

---

## Cara Menggantikan Negative Prompt

DALL-E 3 tidak memiliki negative prompt. Gunakan **framing positif:**

| Ingin Hindari | Cara Framing |
|--------------|-------------|
| `no blurry` | "sharp focus, crystal clear image" |
| `no text/watermark` | "clean image, no text overlay" |
| `no extra limbs` | "anatomically correct, natural pose" |
| `no dark background` | "bright and well-lit environment" |
| `no cartoon` | "photorealistic, photographed" |

---

## Teks dalam Gambar

DALL-E 3 adalah model terbaik untuk teks dalam gambar. Format yang tepat:

```
Include the text "[teks yang diinginkan]" written in [gaya] on [lokasi dalam gambar].
```

**Contoh:**
```
A coffee shop chalkboard menu. Include the text "Daily Special: Cappuccino $4.50" 
written in elegant chalk lettering at the top of the board.
```

---

## Best Practices

### DO ✅
- Tulis dalam kalimat natural yang panjang dan deskriptif
- Sebutkan style referensi dengan konteks ("in the style of...")
- Gunakan kata sifat yang vivid dan spesifik
- Untuk teks: instruksikan secara eksplisit dengan format yang tepat
- Deskripsikan pencahayaan sebagai bagian dari narasi

### DON'T ❌
- Jangan gunakan keyword list pendek
- Jangan gunakan `--parameter` style Midjourney
- Jangan gunakan negative prompt syntax
- Jangan gunakan kata-kata ekstrem yang sering trigger safety filter
- Jangan berharap level artistik yang sama dengan MJ untuk fantasy art

---

## Contoh Prompt per Gaya

### Fotografi Realistis
```
A high-quality photograph of [subjek]. [Detail pakaian/penampilan]. 
[Setting detail]. [Pencahayaan detail]. Shot on [kamera], [lensa], 
[kondisi pencahayaan]. Professional photography, sharp focus.
```

### Digital Art / Ilustrasi
```
A detailed digital illustration of [subjek]. [Deskripsi detail]. 
[Setting]. [Mood dan atmosfer]. Art style: [referensi style]. 
[Karakteristik visual khas]. High-resolution digital artwork.
```

### Lukisan Tradisional
```
An oil painting depicting [subjek dan aksi]. [Detail setting]. 
[Pencahayaan dan warna dominan]. Painted in the style of [referensi artis/era]. 
[Karakteristik brush stroke atau teknik]. Museum-quality fine art.
```

---

## Aspek Rasio

DALL-E 3 mendukung tiga ukuran:
- **1024×1024** — Square (1:1)
- **1792×1024** — Landscape (16:9 approx)
- **1024×1792** — Portrait (9:16 approx)

Sebutkan di dalam prompt: "in landscape format" atau "in portrait orientation"

---

*Update dokumen ini jika OpenAI merilis fitur/model baru*

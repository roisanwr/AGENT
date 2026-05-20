# Knowledge: Midjourney v6+

| Field | Value |
|-------|-------|
| **Model** | Midjourney v6.1 / Niji 6 |
| **Kategori** | Image Generation |
| **Versi Dokumen** | 1.0 |
| **Terakhir Update** | Mei 2026 |
| **Platform** | midjourney.com, Discord |

---

## Karakteristik Model

**Kekuatan:**
- Style artistik yang sangat beragam dan konsisten
- Kualitas estetika tinggi — hasil sering terlihat "indah secara natural"
- Sangat responsif terhadap referensi style artis
- Niji mode: terbaik untuk anime/manga/ilustrasi Jepang
- Omni Reference untuk konsistensi karakter dan style antar gambar

**Kelemahan:**
- Teks dalam gambar masih tidak akurat
- Anatomi tangan masih bisa bermasalah
- Kontrol pixel-level terbatas
- Memerlukan Discord atau akun berbayar

**Sensitivitas Kata:**
- MJ membaca token dari **awal ke akhir** — subjek di awal = lebih dominan
- Kata-kata awal memiliki bobot lebih besar
- Daftar kata yang dipisah koma = lebih fleksibel dari kalimat naratif

---

## Sintaks & Parameter

### Parameter Utama

| Parameter | Fungsi | Nilai | Contoh |
|-----------|--------|-------|--------|
| `--ar` | Aspect ratio | 1:1, 16:9, 9:16, 4:3, 3:2, dll | `--ar 16:9` |
| `--style` | Style rendering | `raw`, `cute`, `expressive`, `scenic` | `--style raw` |
| `--stylize` / `--s` | Kekuatan artistik | 0-1000 (default: 100) | `--s 750` |
| `--chaos` / `--c` | Variasi hasil | 0-100 (default: 0) | `--c 30` |
| `--weird` / `--w` | Keunikan/ketidakterdugaan | 0-3000 | `--w 500` |
| `--no` | Negative prompt | kata yang tidak diinginkan | `--no blur, watermark` |
| `--quality` / `--q` | Kualitas render | 0.25, 0.5, 1 (default: 1) | `--q 1` |
| `--seed` | Reproduksi hasil | angka acak | `--seed 1234` |
| `--tile` | Pattern tileable | tidak ada nilai | `--tile` |
| `--niji` | Mode anime | `--niji 6` | `--niji 6` |

### Parameter Tambahan (Advanced)

| Parameter | Fungsi |
|-----------|--------|
| `--iw` | Image weight (saat pakai image reference, 0.5-2) |
| `--cref [URL]` | Character reference — kunci wajah/karakter |
| `--sref [URL]` | Style reference — tiru gaya gambar referensi |
| `--oref [URL]` | Omni reference — kombinasi karakter + style |

---

## Struktur Prompt Optimal

**Urutan yang direkomendasikan:**
```
[subjek detail] [action/pose] [setting/environment] [lighting] [style/medium] [mood/atmosphere] [quality boosters] [parameters]
```

**Contoh prompt berkualitas tinggi:**
```
A fierce female warrior with silver hair and battle scars, standing on a cliff overlooking a stormy ocean, dramatic rim lighting from the setting sun, cinematic oil painting style, epic fantasy atmosphere, highly detailed, masterpiece --ar 2:3 --style raw --s 500 --no blur, watermark, extra limbs
```

**Contoh prompt sederhana tapi efektif:**
```
Cute panda eating bamboo in a bamboo forest, Studio Ghibli style, soft watercolor, golden hour lighting, dreamy atmosphere --ar 1:1 --niji 6
```

---

## Best Practices

### DO ✅
- Letakkan subjek utama di awal prompt
- Sebutkan lighting secara eksplisit
- Gunakan `--no` untuk elemen yang tidak diinginkan
- Gunakan `--style raw` untuk kontrol lebih besar atas hasil
- Tingkatkan `--s` (200-700) untuk hasil yang lebih artistik
- Gunakan `--cref` untuk konsistensi karakter

### DON'T ❌
- Jangan campur style yang kontradiksi tanpa intensi
- Jangan buat prompt yang terlalu panjang (idealnya < 60 kata untuk body prompt)
- Jangan berharap teks dalam gambar akurat
- Jangan gunakan `--chaos` tinggi jika ingin hasil yang spesifik
- Hindari kata-kata yang abstrak sebagai elemen utama

---

## Kata/Frasa yang Sering Meningkatkan Kualitas

```
masterpiece, best quality, highly detailed, award-winning photography,
cinematic lighting, sharp focus, intricate details, professional,
8K, ultra-realistic, stunning, breathtaking
```

## Kata/Frasa yang Harus Masuk Negative Prompt

```
--no blurry, out of focus, low quality, watermark, text, signature,
bad anatomy, extra limbs, deformed hands, ugly, worst quality,
cartoon (jika mau realis), realistic (jika mau anime)
```

---

## Contoh Prompt per Kategori

### Portrait / Character
```
[karakter detail: usia, rambut, ekspresi, pakaian], [setting singkat], [pencahayaan spesifik], cinematic portrait photography, shot on Canon EOS R5, bokeh background --ar 2:3 --style raw --s 400
```

### Landscape / Environment
```
[setting detail], [waktu hari], [cuaca/kondisi], [mood], epic fantasy concept art, matte painting style, cinematic composition --ar 16:9 --s 600
```

### Character Design / Concept Art
```
[nama karakter] character design, [deskripsi detail: pakaian, weapon, pose], [style referensi], full body reference sheet, white background --ar 1:1 --style raw
```

### Product / Commercial
```
[nama produk] product photography, [setting/background], [pencahayaan], minimalist style, professional commercial photography, ultra-detailed --ar 4:3 --style raw --s 200
```

---

## Changelog Model

| Versi MJ | Perubahan Penting |
|----------|-----------------|
| v6.1 | Peningkatan konsistensi teks, Omni Reference |
| v6.0 | Kualitas prompt following jauh lebih baik, --style raw |
| Niji 6 | Generasi anime terbaik, konsistensi karakter meningkat |

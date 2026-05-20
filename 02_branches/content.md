# Cabang KONTEN — Rules & Fixed Steps

| Field | Value |
|-------|-------|
| **Nama Cabang** | `content` |
| **Label** | Konten / Tulisan / Script |
| **Cakupan** | Artikel, caption, copywriting, email, blog, script video/podcast, newsletter |
| **Target Model** | Claude, GPT-4o/5, Gemini |
| **Versi** | 1.0 |

---

## Fixed Steps — Parameter yang Digali

### Step 1 — Format & Panjang
- **Pertanyaan:** *"Format konten dan panjangnya?"*
- **Pilihan:**
  - `Artikel blog: 500 / 800 / 1500 / 2500+ kata`
  - `Caption media sosial: pendek (50-100 kata) / sedang (100-200) / panjang (200-300)`
  - `Script video/podcast: 60 detik / 3 menit / 5 menit / 10 menit`
  - `Email marketing: satu paragraf / standar / panjang`
  - `Thread Twitter: 5-10 tweet`
  - `Newsletter: 300 / 600 / 1000 kata`
  - *(atau ketik sendiri)*
- **Default:** tidak ada — WAJIB
- **Variabel:** `@content_format`

### Step 2 — Platform & Audiens
- **Pertanyaan:** *"Di mana konten ini akan dipublikasikan dan untuk siapa?"*
- **Platform:** `Instagram / LinkedIn / YouTube / TikTok / Twitter / Blog / Email / Podcast`
- **Audiens:** Minta user spesifikkan: "pemula di bidang X usia Y-Z" bukan "orang umum"
- **Default:** tidak ada — WAJIB
- **Variabel:** `@content_platform`, `@content_audience`

### Step 3 — Tujuan Konten
- **Pertanyaan:** *"Apa tujuan utama konten ini?"*
- **Pilihan:**
  - `Edukasi — ajarkan sesuatu`
  - `Entertainment — hibur dan engage`
  - `Konversi — dorong action (beli, daftar, klik)`
  - `Awareness — perkenalkan brand/produk`
  - `Engagement — pancing komentar dan share`
  - `Loyalitas — perkuat hubungan dengan existing audience`
- **Default:** `Edukasi`
- **Variabel:** `@content_goal`

### Step 4 — Tone & Gaya Bahasa
- **Pertanyaan:** *"Bagaimana tone-nya? Deskripsikan secara spesifik — bukan hanya 'profesional'."*
- **Panduan:** Minta user bayangkan bagaimana seseorang yang mereka kagumi menulis
- **Contoh baik:** "seperti konsultan yang bicara ke teman lama — hangat tapi berbobot, tidak kaku, tidak pakai jargon corporate"
- **Default:** `informatif dan conversational`
- **Variabel:** `@content_tone`

### Step 5 — Topik & Angle
- **Pertanyaan:** *"Apa topik spesifiknya? Apakah kamu sudah punya angle/hook tertentu?"*
- **Panduan:** Angle adalah pembeda antara konten biasa dan konten yang menarik
- **Contoh angle:** "bukan sekadar tips X, tapi kenapa kebanyakan orang salah memahami X"
- **Default:** minta agent generate 3 pilihan angle untuk user pilih
- **Variabel:** `@content_topic`, `@content_angle`

---

## Struktur Prompt Output

```
[format + panjang] + [platform] + [audiens spesifik] + [tujuan konten] + [tone + gaya] + [topik + angle] + [hal yang dihindari]
```

**Template Artikel Blog:**
```
Tulis artikel blog {@content_format} untuk platform {@content_platform}.
Target audiens: {@content_audience}
Tujuan: {@content_goal}
Tone: {@content_tone}
Topik: {@content_topic}
Angle: {@content_angle}
Struktur: intro yang hook + body dengan subheading + CTA yang jelas
Hindari: jargon, kalimat pasif berlebihan, kesimpulan yang klise
```

**Template Caption Media Sosial:**
```
Buat caption {@platform} {@content_format}.
Audiens: {@content_audience}
Tujuan: {@content_goal}
Tone: {@content_tone}
Hook pertama: harus menarik perhatian dalam 2 baris pertama
Topik: {@content_topic}
Angle: {@content_angle}
Sertakan: emoji yang relevan, CTA di akhir, hashtag suggestions
```

**Template Script Video/Podcast:**
```
Tulis script video/podcast durasi {@durasi} untuk {@platform}.
Audiens: {@content_audience}
Tujuan: {@content_goal}
Tone: {@content_tone}
Struktur: hook (15 detik) + intro + body + outro + CTA
Topik: {@content_topic}
Angle: {@content_angle}
Gaya bahasa: conversational, seperti berbicara langsung ke penonton
```

---

## Quality Checklist

**WAJIB:**
```
□ Format dan panjang eksplisit (tanpa ini model pilih sendiri — biasanya tidak pas)
□ Audiens sudah spesifik (bukan "orang umum")
□ Tujuan konten sudah ditentukan
□ Platform sudah disebutkan
```

**SANGAT DIREKOMENDASIKAN:**
```
□ Tone tidak abstrak (deskripsikan seperti menulis ke teman vs akademis, dll)
□ Ada angle/hook yang sudah dipikirkan
□ Ada hal-hal yang harus dihindari
□ CTA sudah ditentukan untuk konten marketing
```

---

## Jebakan Umum & Solusinya

**Jebakan 1: Tone yang tidak terdefinisi**
> "Tone profesional" → masih terlalu luas
> **Solusi:** Minta user deskripsikan: "seperti siapa bicara ke siapa dalam konteks apa?"

**Jebakan 2: Tidak menyebut hal yang dihindari**
> Model tidak bisa menebak pantangan tanpa instruksi
> **Solusi:** Selalu tanyakan "ada kata/topik/pendekatan yang tidak boleh muncul?"

**Jebakan 3: Platform tidak disebutkan**
> Gaya Instagram ≠ LinkedIn ≠ YouTube ≠ Twitter — sangat berbeda
> **Solusi:** Platform adalah parameter wajib di Step 2

**Jebakan 4: Audiens terlalu generik**
> "Semua orang" atau "anak muda" → tidak ada fokus
> **Solusi:** Dorong: "pemula di bidang X usia Y-Z yang sedang mencari Z"

---

## System Prompt Sub-Agent

```
---SYSTEM PROMPT SUB-AGENT KONTEN---

Kamu adalah specialist content strategy dan copywriting prompt engineer. Kamu tahu bahwa konten yang bagus selalu dimulai dari kejelasan tentang SIAPA yang akan membacanya, DI MANA, dan UNTUK APA.

Parameter yang kamu gali:
1. Format + panjang (WAJIB — tanpa ini output tidak bisa dikontrol)
2. Platform + audiens spesifik (WAJIB)
3. Tujuan konten (edukasi/konversi/awareness/dll)
4. Tone spesifik — JANGAN terima jawaban abstrak
5. Topik + angle/hook

ATURAN KHUSUS KONTEN:
- Tone "profesional" atau "friendly" TIDAK cukup — minta user spesifikkan
- Angle adalah pembeda konten biasa vs konten yang menarik — selalu tanyakan
- Jika user tidak punya angle: generate 3 pilihan angle untuk dipilih
- Selalu tanyakan hal yang harus dihindari — ini sering dilupakan
- Platform sangat mempengaruhi gaya: LinkedIn ≠ Instagram ≠ YouTube

---SYSTEM PROMPT SELESAI---
```

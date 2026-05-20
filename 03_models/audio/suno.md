# Knowledge: Suno v4

| Field | Value |
|-------|-------|
| **Model** | Suno v4 |
| **Kategori** | Audio / Music Generation |
| **Versi Dokumen** | 1.0 |
| **Terakhir Update** | Mei 2026 |
| **Platform** | suno.com |

---

## Karakteristik Model

**Kekuatan:**
- Full song generation — verse, chorus, bridge, outro
- Kualitas vokal yang sangat natural
- Pemahaman genre yang luas
- Konsistensi kualitas produksi tinggi
- Mode custom: bisa input lirik dan style sendiri

**Kelemahan:**
- Style prompt > 200 karakter sering diabaikan sebagian
- Genre yang terlalu campur menghasilkan output random
- Lirik yang panjang tanpa structure tag → struktur lagu acak
- Kontrol atas instrumen individual terbatas

---

## Dua Mode Penggunaan

### Mode 1: Simple (Style + Lirik Auto)
```
Input: Style description saja
Output: Suno generate lirik + musik otomatis
Cocok untuk: Eksplorasi cepat, tidak butuh lirik spesifik
```

### Mode 2: Custom (Lirik + Style Manual)
```
Input: Style description + lirik lengkap dengan section tags
Output: Musik mengikuti lirik dan style yang diberikan
Cocok untuk: Lagu dengan pesan spesifik, konten branded
```

---

## Format Style Prompt

**Formula "The Anchor":**
```
[Genre/Style] + [Tempo/Energy] + [Instrumentation] + [Mood/Emotion]
```

**Aturan kritis:**
- **Maksimal 200 karakter** (termasuk spasi)
- Pisahkan elemen dengan koma
- Urutkan dari yang paling penting ke yang kurang penting

**Contoh style prompt yang baik:**
```
indie pop, upbeat 120 BPM, acoustic guitar + synth, nostalgic and warm, summer vibes
```

**Contoh yang terlalu panjang (harus dipotong):**
```
❌ indie pop, upbeat 120 BPM, acoustic guitar + synth, nostalgic and warm, summer vibes, female vocalist, harmonies, reverb-heavy production, inspired by Taylor Swift early albums
```

**Versi yang dipotong dengan tepat:**
```
✅ indie pop, upbeat, acoustic guitar + synth, nostalgic, female vocals, summer vibes
```

---

## Format Lirik & Section Tags

**Section tags yang didukung Suno v4:**
```
[Intro]          → Bagian pembuka instrumental
[Verse]          → Bait utama
[Pre-Chorus]     → Bridge menuju chorus
[Chorus]         → Reff yang catchy
[Bridge]         → Bagian berbeda di tengah/akhir
[Outro]          → Penutup
[Hook]           → Fragment yang sangat catchy
[Instrumental]   → Bagian instrumental saja
[Break]          → Breakbeat/jeda
[Spoken]         → Dialog atau spoken word
```

**Template struktur lagu lengkap:**
```
[Intro]

[Verse 1]
{bait pertama — setup cerita/tema}

[Pre-Chorus]
{build-up menuju chorus}

[Chorus]
{hook utama yang catchy — harus berulang}

[Verse 2]
{bait kedua — lanjutkan cerita}

[Pre-Chorus]
{sama atau variasi}

[Chorus]
{ulangi chorus}

[Bridge]
{bagian berbeda — emotional peak atau plot twist}

[Chorus]
{ulangi chorus, bisa dengan variasi}

[Outro]
{penutup — bisa reprise verse atau fade}
```

---

## Best Practices

### DO ✅
- Keep style prompt di bawah 200 karakter
- Gunakan section tags untuk SEMUA lirik yang diinput
- Genre + sub-genre: spesifik (bukan hanya "pop")
- Sebutkan apakah vokal pria/wanita jika penting
- Tambahkan mood/emotion — Suno sangat responsif terhadap ini
- Gunakan kata-kata seperti "catchy", "anthemic", "melancholic" untuk hasil yang lebih tepat

### DON'T ❌
- Jangan stack terlalu banyak genre yang tidak kompatibel
- Jangan input lirik panjang tanpa section tags
- Jangan referensi artis tanpa deskripsikan elemen musiknya
- Jangan berharap kontrol atas instrumen individual secara presisi
- Jangan buat chorus yang terlalu panjang (Suno lebih baik dengan chorus pendek dan catchy)

---

## Contoh Prompt Lengkap

### Lagu Upbeat untuk Konten YouTube

**Style:**
```
indie pop, upbeat 125 BPM, acoustic guitar + drums + synth pad, energetic and positive, male vocals
```

**Lirik:**
```
[Intro]

[Verse 1]
Every morning starts the same
Coffee hot, a new day's game
Goals I set, dreams I chase
Moving forward at my pace

[Pre-Chorus]
And I know that nothing comes easy
But I'm ready for the climb

[Chorus]
Rise up, rise up, shine your light
Make today worth every fight
Rise up, rise up, give your all
Stand back up whenever you fall

[Verse 2]
Screens and keyboards, endless scrolls
Finding ways to reach your goals
Late nights paying off today
Watch me do it my own way

[Bridge]
They said I couldn't make it here
But I turned every doubt to fuel this fire

[Chorus]

[Outro]
```

### Ambient Background Music

**Style:**
```
lo-fi ambient, slow 70 BPM, piano + soft pads, peaceful and focused, instrumental only
```

*(Tidak perlu lirik — biarkan Suno generate secara instrumental)*

---

## Genre Compatibility Guide

| Genre Utama | Kompatibel Dengan | Tidak Kompatibel |
|-------------|------------------|------------------|
| Lo-fi Hip-Hop | Jazz, Ambient, Boom Bap | Heavy Metal, EDM |
| Cinematic | Orchestral, Ambient, Post-Rock | Trap, K-Pop |
| Pop | Indie, R&B, Dance, Electronic | Death Metal, Drone |
| Jazz | Bossa Nova, Smooth, Nu-Jazz | Trap, Dubstep |
| EDM | House, Techno, Trance | Acoustic, Classical |

---

*Update: Verifikasi kompatibilitas genre setelah setiap major Suno update*

# Knowledge: Google Veo 2/3

| Field | Value |
|-------|-------|
| **Model** | Google Veo 2 / Veo 3 |
| **Kategori** | Video Generation |
| **Versi Dokumen** | 1.0 |
| **Terakhir Update** | Mei 2026 |
| **Platform** | Google Labs, VideoFX, Gemini Ultra |

---

## Karakteristik Model

**Kekuatan:**
- Kualitas sinematik tertinggi di kelasnya
- Motion fidelity — gerak yang sangat natural dan believable
- Pemahaman instruksi kamera yang sangat baik
- Veo 3: bisa generate audio bersamaan dengan video
- Konsistensi visual dalam satu clip

**Kelemahan:**
- Akses terbatas (Google Labs / Gemini Ultra)
- Multi-scene dalam satu prompt masih terbatas
- Character consistency antar clip perlu "anchor tokens"
- Prompt yang terlalu kompleks bisa menghasilkan hasil yang mixed

**Filosofi Prompting Veo:**
> Think like a **Director of Photography (DP)**.  
> Kamu sedang memberikan brief kepada kameramen profesional.

---

## Formula Prompt Veo

```
[Camera Shot + Movement] + [Subject + Action] + [Environment/Setting] + [Visual Style + Lighting] + [Audio (Veo 3)]
```

**Contoh lengkap:**
```
Slow dolly-in tracking shot. A young woman in a red dress stands alone in an empty 
ballroom, slowly turning to face the camera. The room is dimly lit with warm chandelier 
light casting long shadows on the marble floor. Cinematic, film grain texture, 
melancholic atmosphere. Subtle ambient sound of an empty room.
```

---

## Camera Vocabulary

**Shot Types:**
| Terminologi | Arti |
|-------------|------|
| `Extreme Wide Shot (EWS)` | Seluruh environment, subjek sangat kecil |
| `Wide Shot (WS)` | Seluruh subjek, environment terlihat |
| `Medium Shot (MS)` | Pinggang ke atas |
| `Close-Up (CU)` | Wajah atau objek detail |
| `Extreme Close-Up (ECU)` | Detail sangat kecil |
| `Bird's Eye View` | Tampak langsung dari atas |
| `Worm's Eye View` | Tampak langsung dari bawah |
| `POV Shot` | Sudut pandang orang pertama |

**Camera Movements:**
| Terminologi | Deskripsi |
|-------------|-----------|
| `Dolly in/out` | Kamera maju/mundur secara fisik |
| `Pan left/right` | Kamera berputar horizontal |
| `Tilt up/down` | Kamera berputar vertikal |
| `Tracking shot` | Kamera mengikuti gerakan subjek |
| `Crane shot` | Gerakan kamera dari bawah ke atas |
| `Aerial drone shot` | Tampak dari udara |
| `Handheld` | Gerakan kamera yang sedikit goyah (natural) |
| `Steadicam` | Gerakan smooth meski berjalan |
| `360 orbit` | Kamera berputar mengelilingi subjek |
| `Zoom in/out` | Lensa zoom (bukan gerakan fisik kamera) |

---

## Lighting Vocabulary

```
golden hour — cahaya hangat saat matahari terbenam/terbit
magic hour — transisi langit yang dramatis
soft diffused light — cahaya merata, tanpa bayangan keras
dramatic side lighting — bayangan kuat dari samping
backlit / silhouette — cahaya dari belakang, subjek gelap
rim light — garis cahaya di tepi subjek
neon glow — cahaya warna-warni dari neon sign
moonlit — cahaya bulan, kebiruan
overcast — mendung, cahaya flat dan merata
studio lighting — tiga titik cahaya professional
```

---

## Visual Style Reference

```
cinematic — seperti film Hollywood modern
film grain — tekstur granular seperti film analog
documentary style — handheld, natural, journalistic
anime aesthetic — gaya animasi Jepang
hyperrealistic — foto yang sangat detail dan nyata
surrealist — elemen tidak nyata yang believable
black and white — tanpa warna, fokus pada kontras
color graded: teal and orange — warna khas blockbuster
noir — kontras tinggi, bayangan dalam, misterius
```

---

## Anchor Tokens untuk Konsistensi

Ketika ingin konsistensi karakter/objek antar clip, gunakan "anchor tokens" — deskripsi yang sama persis:

```
Clip 1: "A woman with curly auburn hair wearing a matte black leather jacket..."
Clip 2: "The same woman with curly auburn hair and matte black leather jacket..."
Clip 3: "She — with her curly auburn hair and matte black leather jacket..."
```

**Anchor tokens yang efektif:**
- Warna dan material pakaian
- Ciri khas fisik yang unik
- Objek yang dibawa karakter
- Material/tekstur environment yang spesifik

---

## Audio (Khusus Veo 3)

Veo 3 mampu generate audio bersamaan. Tambahkan di akhir prompt:

```
[ambient sound description]: Include the sound of [deskripsi suara ambient]
[music]: Gentle [genre] music playing in the background
[dialogue]: Character says "[dialog yang diinginkan]"
[sound effects]: Include [deskripsi efek suara]
```

**Contoh:**
```
...Include ambient sound of rain on windows, distant thunder, and a crackling fireplace.
```

---

## Best Practices

### DO ✅
- Mulai selalu dengan camera shot type + movement
- Deskripsikan aksi secara temporal dan berurutan
- Sebutkan lighting secara eksplisit
- Gunakan anchor tokens untuk konsistensi karakter
- Mulai dari single-subject scene sebelum mencoba kompleks

### DON'T ❌
- Jangan coba narrate multiple scenes dalam satu prompt
- Jangan lupakan camera movement — ini yang paling sering dilupakan
- Jangan gunakan deskripsi statis (tidak ada gerak/aksi)
- Jangan terlalu banyak elemen dalam satu frame

---

## Template Prompt

### Action Scene
```
[Camera]: [shot type] + [movement]
[Action]: [Subjek] + [aksi dari awal hingga akhir]
[Setting]: [environment detail]
[Style]: cinematic, [color palette], [lighting]
[Duration]: [estimasi durasi]
```

### Nature/Landscape
```
Aerial drone shot slowly pulling back. [Deskripsi landscape]. 
[Waktu hari dan kondisi cahaya]. [Elemen yang bergerak: angin, air, dll]. 
[Color grade/style]. Cinematic nature documentary aesthetic.
```

### Character/Story
```
[Camera movement], [durasi action]. [Karakter dengan anchor tokens] + [aksi temporal]. 
[Setting]. [Lighting]. [Atmosfer/mood]. [Audio jika Veo 3].
```

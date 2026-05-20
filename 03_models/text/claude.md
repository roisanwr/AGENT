# Knowledge: Claude (Anthropic)

| Field | Value |
|-------|-------|
| **Model** | Claude 3.7 Sonnet / Haiku / Opus |
| **Kategori** | Teks / Coding / Persona |
| **Versi Dokumen** | 1.0 |
| **Terakhir Update** | Mei 2026 |
| **Platform** | claude.ai, Anthropic API |

---

## Karakteristik Model

**Kekuatan:**
- Mengikuti instruksi panjang dan kompleks dengan sangat akurat
- Nuanced reasoning — bisa menangani nuansa dan ambiguitas
- Persona/roleplay yang konsisten dalam jangka panjang
- Long-form writing dengan kualitas tinggi
- Pemahaman konteks yang sangat baik
- XML tag structure — format terbaik untuk instruksi kompleks

**Kelemahan:**
- Lebih "hati-hati" dibanding GPT dalam beberapa topik
- Bisa verbose jika tidak diberi batasan panjang
- Context window yang terbatas dibanding model tertentu

**Cara Claude "Berpikir":**
> Claude merespons terbaik pada instruksi yang **terstruktur, jelas, dan diprioritaskan**.  
> Claude adalah tentang **kontrak** — tentukan apa yang kamu mau dengan presisi.

---

## Format Prompt Optimal: XML Tags

**Pendekatan terbaik untuk Claude adalah menggunakan XML tags:**

```xml
<instructions>
[Apa yang harus dilakukan — gunakan verb yang jelas]
</instructions>

<context>
[Latar belakang yang relevan]
</context>

<requirements>
[Persyaratan spesifik]
</requirements>

<constraints>
[Batasan yang harus dipenuhi]
</constraints>

<examples>
[1-3 contoh input/output]
</examples>

<output_format>
[Format yang diharapkan untuk output]
</output_format>
```

---

## Struktur System Prompt yang Optimal

```xml
<system>
  <role>
    Kamu adalah [role yang spesifik dengan expertise].
  </role>
  
  <mission>
    [Misi utama dalam satu kalimat]
  </mission>
  
  <rules>
    1. [Aturan pertama]
    2. [Aturan kedua]
    3. [Aturan ketiga]
  </rules>
  
  <tone>
    [Deskripsi tone dan cara berkomunikasi]
  </tone>
  
  <limits>
    [Hal yang TIDAK boleh dilakukan]
  </limits>
</system>
```

---

## Prinsip Prompting untuk Claude

### 1. Gunakan Verb yang Jelas
| Hindari | Gunakan |
|---------|---------|
| "Tolong bantu saya" | "Tulis / Analisis / Buat / Evaluasi" |
| "Sesuatu tentang X" | "Artikel 800 kata tentang X untuk audiens Y" |
| "Lebih bagus" | "Tingkatkan kejelasan dan tambahkan 3 contoh" |

### 2. Examples Over Adjectives
```xml
<!-- Kurang efektif -->
<style>Tulis dengan gaya yang engaging, menarik, dan informatif.</style>

<!-- Lebih efektif -->
<example>
Input: "Apa itu machine learning?"
Output: "Bayangkan kamu punya anjing yang belajar dari pengalaman. Setiap kali 
dia melakukan sesuatu yang benar, kamu kasih hadiah. Lama-kelamaan dia tahu 
apa yang harus dilakukan. Machine learning bekerja persis seperti itu..."
</example>
```

### 3. Framing Positif
```
❌ "Jangan gunakan bahasa formal"
✅ "Gunakan bahasa sehari-hari yang akrab dan conversational"

❌ "Jangan terlalu panjang"
✅ "Maksimal 300 kata per respons"
```

### 4. Socratic Opening (untuk task kompleks)
Untuk task yang ambigu, mulai dengan:
```
Sebelum kamu mulai, apa informasi yang kamu butuhkan dari saya untuk 
menyelesaikan task ini dengan optimal?
```

---

## Template per Use Case

### Coding Prompt
```xml
<instructions>
Kamu adalah senior [bahasa] developer. Buat [deskripsi fungsi].
</instructions>

<requirements>
- Language: [bahasa + versi]
- Framework: [framework jika ada]
- Input: [deskripsi input]
- Output: [deskripsi output yang diharapkan]
- Constraints: [batasan teknis]
- Style: [clean/verbose/production-ready]
</requirements>

<example>
Input: [contoh input]
Expected output: [contoh output]
</example>

<output_format>
Berikan hanya kode, dengan komentar singkat di bagian yang tidak obvious.
</output_format>
```

### Persona / Character Prompt
```xml
<persona>
  <name>[Nama karakter]</name>
  <identity>[Siapa karakter ini dan mengapa ada]</identity>
  <tone>[Cara berbicara yang spesifik]</tone>
  <expertise>[Yang bisa dan tidak bisa dilakukan]</expertise>
  <limits>[Batasan eksplisit]</limits>
</persona>

<examples>
  <qa>
    <q>[Pertanyaan umum user]</q>
    <a>[Respons ideal karakter]</a>
  </qa>
</examples>

<instructions>
Stay in character at all times. If asked about topics outside your expertise, 
redirect politely to what you can help with.
</instructions>
```

### Writing / Content Prompt
```xml
<instructions>
Tulis [format] tentang [topik] dalam gaya [tone description].
</instructions>

<context>
Target audiens: [deskripsi spesifik]
Platform: [platform distribusi]
Tujuan konten: [edukasi/konversi/awareness]
</context>

<requirements>
- Panjang: [jumlah kata/karakter]
- Struktur: [outline atau format yang diinginkan]
- Tone: [deskripsi gaya penulisan spesifik]
- Sertakan: [elemen yang harus ada]
- Hindari: [yang tidak boleh ada]
</requirements>

<output_format>
[Format output: Markdown / plain text / dengan heading, dll]
</output_format>
```

---

## Best Practices

### DO ✅
- Gunakan XML tags untuk memisahkan komponen instruksi
- Berikan 1-3 contoh konkret (few-shot) alih-alih deskripsi panjang
- Sebutkan format output yang diharapkan secara eksplisit
- Gunakan framing positif ("lakukan X") bukan negatif ("jangan lakukan Y")
- Tentukan panjang output jika penting

### DON'T ❌
- Jangan buat system prompt > 2000 kata tanpa alasan kuat
- Jangan stack terlalu banyak instruksi tanpa prioritas
- Jangan gunakan kata sifat abstrak tanpa contoh konkret
- Jangan lupa sebutkan format output yang diharapkan

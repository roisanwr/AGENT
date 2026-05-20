# Knowledge: GPT-4o / GPT-5 (OpenAI)

| Field | Value |
|-------|-------|
| **Model** | GPT-4o, GPT-4.5, GPT-5 (OpenAI) |
| **Kategori** | Teks / Coding / Reasoning |
| **Versi Dokumen** | 1.0 |
| **Terakhir Update** | Mei 2026 |
| **Platform** | ChatGPT, OpenAI API |

---

## Karakteristik Model

**Kekuatan:**
- Reasoning mendalam dan multi-step problem solving
- Coding yang sangat akurat untuk berbagai bahasa
- Multimodal: bisa proses gambar, audio, dan teks
- Memory/Custom Instructions untuk personalisasi persistent
- GPT-5: extended thinking mode untuk masalah kompleks

**Kelemahan:**
- Lebih mahal di API dibanding Claude Haiku atau Gemini Flash
- Bisa "over-explain" jika tidak diberi batasan
- Prompt yang ambigu bisa menghasilkan interpretasi yang tidak diharapkan

**Pendekatan Fundamental:**
> GPT merespons terbaik pada **outcome-oriented prompts** — fokus pada APA yang dihasilkan, bukan HOW.

---

## Format Prompt Optimal

**Untuk task coding/reasoning:**
```
[Konteks singkat 1-2 kalimat]
[Task yang jelas dengan action verb]

Requirements:
- [Syarat 1]
- [Syarat 2]
- [Syarat 3]

[Contoh input/output jika ada]

Output format: [format yang diharapkan]
```

**Untuk creative/writing:**
```
[Role assignment singkat]
[Task + konteks]
[Format output]
[Constraint: panjang, tone, dll]
```

---

## Custom Instructions (ChatGPT)

Manfaatkan Custom Instructions untuk preferensi yang persisten:

**"What would you like ChatGPT to know about you?":**
```
Saya [deskripsi singkat dirimu]. 
Saya bekerja di bidang [bidang].
Level teknis saya: [beginner/intermediate/expert] di [domain].
Bahasa yang saya prefer untuk respons: [bahasa].
```

**"How would you like ChatGPT to respond?":**
```
- Selalu gunakan [bahasa] untuk merespons
- Langsung ke intinya, tanpa basa-basi panjang
- Jika ada kode: gunakan syntax highlighting
- Panjang maksimal: [preferensi]
```

---

## Chain of Thought Prompting

Untuk masalah kompleks, gunakan CoT:

```
Selesaikan masalah ini langkah demi langkah:
[deskripsi masalah]

Pikirkan:
1. Apa yang diketahui
2. Apa yang perlu dicari tahu
3. Pendekatan yang akan digunakan
4. Eksekusi langkah per langkah
5. Verifikasi jawaban
```

---

## Template per Use Case

### Coding
```
You are a senior [language] developer.

Task: [deskripsi singkat dan jelas]

Context:
- Language: [lang + version]
- Framework: [framework]
- Existing code: [context jika ada]

Requirements:
- [req 1]
- [req 2]

Input: [format input]
Output: [format output]

Provide clean, production-ready code with brief comments on non-obvious parts.
```

### Analysis
```
Analyze the following [type of content]:
[content]

Provide:
1. [aspek 1]
2. [aspek 2]
3. [aspek 3]

Format: [bullet points/table/numbered list]
Length: [preferensi panjang]
```

---

## Best Practices

### DO ✅
- Gunakan structured text dengan label jelas (bukan blok prosa panjang)
- Untuk coding: selalu sebutkan bahasa + versi
- Manfaatkan "Reasoning" mode untuk masalah kompleks
- Berikan contoh output yang diharapkan jika mungkin
- Gunakan Custom Instructions untuk preferensi yang selalu sama

### DON'T ❌
- Jangan tumpuk terlalu banyak instruksi dalam satu kalimat
- Jangan lupa tentukan format output (markdown/JSON/plain text)
- Jangan berharap GPT "ingat" percakapan sebelumnya tanpa memory

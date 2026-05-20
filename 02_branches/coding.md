# Cabang CODING — Rules & Fixed Steps

| Field | Value |
|-------|-------|
| **Nama Cabang** | `coding` |
| **Label** | Coding / Development |
| **Target Model** | Claude, GPT-4o/5, Gemini, GitHub Copilot |
| **Versi** | 1.0 |

---

## Fixed Steps — Parameter yang Digali

### Step 1 — Bahasa & Teknologi
- **Pertanyaan:** *"Bahasa pemrograman apa? Versi dan framework-nya?"*
- **Contoh:** "Python 3.11 + FastAPI", "JavaScript ES2024 + React 19", "PHP 8.3 + Laravel 11"
- **Default:** tidak ada — WAJIB
- **Variabel:** `@code_lang`

### Step 2 — Tujuan Fungsi
- **Pertanyaan:** *"Jelaskan dalam SATU kalimat: apa yang harus dilakukan kode ini?"*
- **Panduan:** Dorong user untuk spesifik. "Sistem login" terlalu umum → "Fungsi autentikasi user menggunakan JWT token dengan expiry 24 jam"
- **Default:** tidak ada — WAJIB
- **Variabel:** `@code_goal`

### Step 3 — Input & Output
- **Pertanyaan:** *"Apa input dan output yang diharapkan? Berikan contoh konkret."*
- **Contoh:** "Input: array angka [3,1,4,1,5]. Output: array terurut [1,1,3,4,5]"
- **Default:** tidak ada — parameter sangat kritis
- **Variabel:** `@code_io`

### Step 4 — Constraint & Batasan
- **Pertanyaan:** *"Ada batasan atau syarat khusus yang harus dipenuhi?"*
- **Contoh:** "harus bisa jalan tanpa library eksternal", "max 50 baris kode", "harus handle error", "compatible dengan Python 3.9+"
- **Default:** tidak ada batasan khusus
- **Variabel:** `@code_constraints`

### Step 5 — Gaya & Konteks
- **Pertanyaan:** *"Gaya kode yang diinginkan? Dan apakah ini bagian dari proyek yang lebih besar?"*
- **Pilihan gaya:**
  - `Clean & minimal — kode ringkas, tanpa komentar berlebihan`
  - `Dengan komentar penjelasan — cocok untuk belajar`
  - `Production-ready — include error handling, logging, tests`
  - `Verbose & educational — detail maksimal`
- **Konteks proyek:** Minta user ceritakan struktur/konvensi yang sudah ada jika relevan
- **Default:** `Clean & minimal`
- **Variabel:** `@code_style`

---

## Struktur Prompt Output

**Template untuk Claude:**
```
<instructions>
Kamu adalah senior developer {@code_lang}. Buat {@code_goal}.
</instructions>

<requirements>
- Bahasa: {@code_lang}
- Input: {input dari @code_io}
- Output: {output dari @code_io}
- Constraint: {@code_constraints}
- Style: {@code_style}
</requirements>

<example>
Input: {contoh_input}
Expected output: {contoh_output}
</example>

<context>
{@code_context jika ada}
</context>
```

**Template untuk GPT-4o:**
```
You are a senior {@lang} developer. Your task: {@code_goal}

Requirements:
- Language/Version: {@code_lang}
- Input: {input}
- Output: {output}
- Constraints: {@code_constraints}
- Code style: {@code_style}

Example:
- Input: {contoh_input}
- Output: {contoh_output}

{@code_context jika ada}

Provide only the code with minimal comments unless verbose style is requested.
```

---

## Quality Checklist

**WAJIB:**
```
□ Bahasa pemrograman + versi sudah disebutkan
□ Tujuan fungsi sudah jelas dalam satu kalimat
□ Input dan output sudah ada dengan contoh konkret
□ Target model sudah dipilih
```

**SANGAT DIREKOMENDASIKAN:**
```
□ Constraint sudah didefinisikan
□ Gaya kode sudah dipilih
□ Konteks proyek sudah dijelaskan jika bagian dari sistem lebih besar
```

---

## Jebakan Umum & Solusinya

**Jebakan 1: Tujuan terlalu abstrak**
> "Buatkan sistem login yang aman" → terlalu luas
> **Solusi:** Tanya: auth method apa? Session management bagaimana? Framework apa? Database apa?

**Jebakan 2: Tidak ada contoh input/output**
> Tanpa contoh konkret, model menginterpretasi sendiri → sering meleset
> **Solusi:** Wajibkan contoh di Step 3

**Jebakan 3: Tidak menyebut versi**
> Tanpa versi, model bisa pakai syntax yang sudah deprecated
> **Solusi:** Selalu tanyakan versi di Step 1

**Jebakan 4: Tidak ada konteks proyek**
> Kode yang bagus tapi tidak kompatibel dengan konvensi proyek yang ada
> **Solusi:** Tanyakan di Step 5 apakah ini bagian dari proyek yang lebih besar

---

## System Prompt Sub-Agent

```
---SYSTEM PROMPT SUB-AGENT CODING---

Kamu adalah specialist coding prompt engineer. Kamu tahu bahwa prompt coding yang bagus adalah yang paling presisi dan spesifik.

Parameter yang kamu gali:
1. Bahasa + versi + framework (WAJIB)
2. Tujuan fungsi dalam SATU kalimat yang jelas (WAJIB)
3. Input dan output dengan contoh konkret (SANGAT KRITIS)
4. Constraint dan batasan
5. Gaya kode + konteks proyek

ATURAN KHUSUS CODING:
- Tujuan "satu kalimat" itu penting — kalau lebih dari satu kalimat, itu belum cukup spesifik
- Contoh input/output konkret adalah yang paling mempercepat ketepatan output
- Jika ada kode existing: instruksikan untuk dimodifikasi, bukan ditulis ulang dari nol
- Format prompt berbeda untuk Claude (XML tags) vs GPT (structured text)

---SYSTEM PROMPT SELESAI---
```

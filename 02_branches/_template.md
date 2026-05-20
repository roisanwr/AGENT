# Template Cabang Baru — AGENT

> Salin file ini ketika membuat cabang baru. Isi semua bagian yang ditandai `[ISIAN]`.

---

## Identitas Cabang

| Field | Value |
|-------|-------|
| **Nama Cabang** | `[ISIAN: nama dalam huruf kecil, misal: presentasi]` |
| **Label** | `[ISIAN: nama yang ditampilkan ke user, misal: Presentasi / Slide]` |
| **Deskripsi** | `[ISIAN: satu kalimat menjelaskan cabang ini]` |
| **Versi** | 1.0 |
| **Tanggal Dibuat** | `[ISIAN: tanggal]` |

---

## Kata Kunci Deteksi

> List kata kunci yang memicu routing ke cabang ini

**Kata kunci kuat:**
```
[ISIAN: kata kunci yang hampir pasti menunjukkan cabang ini]
```

**Kata kunci pendukung:**
```
[ISIAN: kata kunci yang mendukung klasifikasi ke cabang ini]
```

**Frasa umum user:**
```
[ISIAN: frasa lengkap yang sering diucapkan user untuk kebutuhan ini]
```

**Ambigu dengan cabang lain:**
```
[ISIAN: kata/frasa yang bisa ambigu dengan cabang lain dan cara resolusinya]
```

---

## Target Model AI

| Model | Kategori | Cocok Untuk |
|-------|---------|------------|
| `[ISIAN]` | `[ISIAN]` | `[ISIAN]` |
| `[ISIAN]` | `[ISIAN]` | `[ISIAN]` |

> Link ke knowledge spesifik model: `../03_models/[kategori]/[model].md`

---

## Parameter yang Digali (Fixed Steps)

Urutan parameter yang akan ditanyakan sub-agent, dari yang paling penting:

### Step 1 — [ISIAN: nama parameter]
- **Pertanyaan ke user:** `"[ISIAN: pertanyaan yang ditampilkan]"`
- **Tipe data:** `[teks bebas / pilihan ganda / angka]`
- **Pilihan yang ditawarkan:** `[ISIAN: jika pilihan ganda]`
- **Default jika tidak dijawab:** `[ISIAN: nilai default]`
- **Nama variabel output:** `@[nama_cabang]_[parameter]`

### Step 2 — [ISIAN: nama parameter]
- **Pertanyaan ke user:** `"[ISIAN]"`
- **Tipe data:** `[ISIAN]`
- **Pilihan yang ditawarkan:** `[ISIAN]`
- **Default jika tidak dijawab:** `[ISIAN]`
- **Nama variabel output:** `@[nama_cabang]_[parameter]`

### Step 3 — [ISIAN: nama parameter]
*(Tambah step sesuai kebutuhan)*

---

## Quality Checklist

Parameter **WAJIB** sebelum generate:
```
□ [ISIAN: parameter 1]
□ [ISIAN: parameter 2]
□ [ISIAN: parameter 3]
```

Parameter **SANGAT DIREKOMENDASIKAN:**
```
□ [ISIAN: parameter opsional 1]
□ [ISIAN: parameter opsional 2]
```

---

## Struktur Prompt Output

Template struktur prompt yang dihasilkan:
```
[ISIAN: komponen 1] + [ISIAN: komponen 2] + [ISIAN: komponen 3] + [ISIAN: ...]
```

**Contoh prompt output yang baik:**
```
[ISIAN: contoh prompt yang dihasilkan cabang ini]
```

---

## Jebakan Umum

> Masalah yang sering muncul pada cabang ini dan cara menghindarinya

**Jebakan 1: [ISIAN: nama masalah]**
[ISIAN: deskripsi masalah dan cara resolusinya]

**Jebakan 2: [ISIAN: nama masalah]**
[ISIAN: deskripsi masalah dan cara resolusinya]

---

## System Prompt untuk Sub-Agent

```
---SYSTEM PROMPT SUB-AGENT [NAMA CABANG HURUF BESAR]---

Kamu adalah specialist untuk membuat prompt [ISIAN: jenis konten] yang berkualitas tinggi.

Tugasmu: Menggali semua parameter yang dibutuhkan untuk generate prompt [ISIAN: jenis] yang optimal untuk model [ISIAN: model target].

## PARAMETER YANG HARUS KAMU GALI

[ISIAN: list parameter + penjelasan mengapa penting]

## CARA BERTANYA

- Tanya SATU parameter per giliran
- Berikan pilihan/contoh untuk setiap pertanyaan
- Jika user tidak menjawab, gunakan default yang masuk akal
- Jangan lanjut ke generate sebelum semua parameter WAJIB terkumpul

## KONTEKS DARI CLASSIFIER

Baca @classifier_output untuk mendapatkan catatan awal dari classifier tentang user ini.

## FORMAT OUTPUT

Setelah semua parameter terkumpul, output dalam format:
PARAMETER_TERKUMPUL:
- [nama_param]: [nilai]
- [nama_param]: [nilai]
SIAP_GENERATE: true

---SYSTEM PROMPT SELESAI---
```

---

## Checklist Sebelum Publish Cabang Baru

```
□ Kata kunci deteksi sudah didefinisikan
□ Ambiguitas dengan cabang lain sudah dipetakan
□ Semua fixed steps sudah ada (minimal 3 step)
□ Default untuk setiap parameter sudah ditentukan
□ Quality checklist sudah dibuat
□ System prompt sub-agent sudah ditulis
□ Knowledge model sudah ada di 03_models/
□ Kondisi routing sudah ditambahkan di Opal Dynamic Router
□ Entri baru ditambahkan ke 01_core/classifier_rules.md
□ Entri baru ditambahkan ke 03_models/_index.md
□ Diuji dengan minimal 5 input berbeda
```

---

*Hapus bagian ini setelah file diisi lengkap*

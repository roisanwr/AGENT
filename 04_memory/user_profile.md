# Format Profil User — AGENT

> Dokumen ini menjelaskan apa yang tersimpan dalam profil user dan bagaimana agent menggunakannya.

---

## Tujuan Profil User

Profil user memungkinkan sistem untuk:
1. **Mempersingkat sesi** — tidak perlu tanya ulang preferensi yang sudah diketahui
2. **Personalisasi rekomendasi** — "Terakhir kamu pakai Midjourney, mau pakai lagi?"
3. **Meningkatkan kualitas** — mengetahui style dan tone yang disukai user

---

## Data yang Disimpan

### Data Fundamental
```
user_id        → Identitas unik (auto-generated)
display_name   → Nama panggilan
created_at     → Tanggal bergabung
last_active    → Aktivitas terakhir
```

### Preferensi per Kategori
```
favorite_category        → Kategori yang paling sering digunakan
favorite_image_model     → Model gambar favorit
favorite_video_model     → Model video favorit
favorite_audio_model     → Model audio favorit
favorite_text_model      → Model teks/coding favorit
```

### Preferensi Gaya
```
preferred_image_style    → Gaya gambar yang sering diminta
preferred_image_ratio    → Aspect ratio yang paling sering dipakai
preferred_content_tone   → Tone penulisan yang disukai
preferred_language       → Bahasa output
```

---

## Cara Agent Menggunakan Profil

### Di Awal Sesi (Personalisasi Greeting)
```
Jika profil ditemukan:
"Halo [display_name]! Terakhir kamu buat [last_category]. Mau lanjut ke kategori itu atau coba yang lain?"

Jika profil baru:
"Halo! Aku AGENT, assistant untuk bikin prompt AI yang berkualitas. Apa yang ingin kamu buat hari ini?"
```

### Saat Routing (Skip Klarifikasi)
```
Jika user input: "buat gambar" (ambigu tapi biasanya GAMBAR)
Dan profil menunjukkan: favorite_category = gambar

→ Langsung routing ke GAMBAR tanpa klarifikasi
→ Sebutkan: "Langsung ke gambar ya, sesuai kebiasaanmu!"
```

### Saat Pilih Model (Rekomendasi Cerdas)
```
Jika user minta gambar dan profil punya: favorite_image_model = midjourney
→ "Rekomendasiku Midjourney — biasanya kamu pakai ini. Mau pakai atau coba yang lain?"
```

### Saat Generate (Gunakan Default yang Dipersonalisasi)
```
Jika preferred_image_ratio = "16:9" tersimpan
→ Gunakan 16:9 sebagai default tanpa perlu tanya lagi
→ Sebutkan: "Pakai 16:9 seperti biasanya"
```

---

## Pembaruan Profil

Profil diperbarui otomatis di setiap akhir sesi:

| Data | Kapan Diperbarui |
|------|-----------------|
| `last_active` | Setiap sesi |
| `favorite_category` | Jika kategori ini sudah dipakai 3+ kali |
| `favorite_[kategori]_model` | Setiap kali user pilih model di kategori itu |
| `preferred_image_style` | Jika user minta style spesifik |
| `preferred_image_ratio` | Jika user pilih ratio yang sama 2x berturut-turut |

---

## Privasi

- Profil disimpan di Google Sheets yang kamu kontrol sendiri
- Tidak ada data yang dikirim ke pihak ketiga
- User bisa minta reset profil kapan saja

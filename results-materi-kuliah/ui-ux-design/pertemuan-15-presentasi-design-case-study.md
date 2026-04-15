# Pertemuan 15: Presentasi Final — Design Case Study

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menyusun Design Case Study yang menceritakan proses desain end-to-end
* Mempresentasikan prototipe Figma secara langsung dan persuasif
* Menjawab pertanyaan teknis tentang keputusan desain dengan confidence
* Memformat proyek sebagai portofolio entry yang siap dilihat rekruter

---

## 📖 Pengantar (Hook)

Dua kandidat untuk posisi UI/UX Designer melamar ke startup fintech.

Kandidat A: "Saya buat aplikasi transfer uang. Ini prototipenya."

Kandidat B: "Saya buat aplikasi transfer untuk ibu rumah tangga yang tidak familiar dengan digital. Kami mulai dengan 15 user interview yang menemukan bahwa mereka takut salah kirim. Ini adalah journey map yang menunjukkan di mana mereka paling frustrasi. Desain kami mengatasi pain point itu dengan [solusi spesifik], dan setelah usability testing dengan 5 pengguna, task completion rate naik dari 55% ke 88%."

Siapa yang dipanggil untuk interview? Kandidat B — setiap kali.

**Design Case Study bukan tentang "apa yang kamu buat" — tapi "bagaimana dan mengapa kamu membuat keputusan itu."**

---

## 🧩 Struktur Design Case Study

### Format Standar (8 Bagian)

**1. Overview / Problem Statement**
```
- Apa masalah yang dipecahkan?
- Siapa target pengguna?
- Apa metrics keberhasilan?
- Timeline proyek

Format ringkas:
"[Nama proyek] adalah [jenis produk] untuk [target pengguna]
yang [pain point utama]. Kami berhasil [outcome terukur]."

Contoh:
"PayKos adalah aplikasi pembayaran kos digital untuk mahasiswa
yang sering lupa bayar sewa karena tidak ada sistem reminder.
Kami berhasil meningkatkan task completion rate pembayaran 
dari 62% ke 91% melalui redesign alur notifikasi."
```

**2. Research & Discovery**
* Metode: wawancara (berapa orang?), survei (berapa responden?), observasi
* Key insights yang mengejutkan atau counterintuitive
* Affinity diagram atau temuan utama

**3. Define: Persona & Journey Map**
* 1-2 persona dengan data yang mendukung
* Journey Map untuk skenario utama
* Pain points yang menjadi fokus desain

**4. Information Architecture**
* Sitemap
* User flow untuk task utama
* Keputusan IA yang diambil dan alasannya

**5. Design Iterations**
* Lo-Fi sketsa → Mid-Fi wireframe → Hi-Fi
* Jelaskan perubahan yang terjadi dari satu iterasi ke berikutnya
* Mengapa versi awal tidak berhasil?

**6. Design System**
* Color palette dan pilihan font
* 3-5 komponen utama
* Bagaimana design system menjaga konsistensi

**7. Usability Testing & Results**
* Metodologi: berapa partisipan, bagaimana prosesnya
* SUS Score sebelum dan sesudah iterasi
* 3 temuan kritis dan bagaimana ditangani

**8. Final Design & Outcomes**
* Demo prototipe hi-fi
* Metrics (SUS score final, task completion rate)
* Lessons learned
* Next steps (apa yang akan dilakukan jika ada waktu lebih?)

---

## 🧠 Prinsip Storytelling Desain

### Show, Don't Just Tell

```
❌ LEMAH:
"Kami melakukan riset pengguna dan menemukan masalah."

✅ KUAT:
"Dari 12 wawancara dengan mahasiswa kost di Bandung, kami 
menemukan satu pola yang mengejutkan: 9 dari 12 tidak 
membayar kos tepat waktu bukan karena tidak punya uang — 
tapi karena LUPA. Quote yang paling sering muncul: 
'Kalau tidak ada yang nagih, aku tidak ingat.'"
```

### Sertakan Angka

```
❌ TANPA DATA:
"Usability testing menunjukkan desain kami lebih baik."

✅ DENGAN DATA:
"SUS Score naik dari 58 ke 74 setelah iterasi.
Task completion rate untuk flow pembayaran: 62% → 91%."
```

### Jelaskan Trade-offs

```
❌ TANPA KONTEKS:
"Kami memilih tab bar navigation."

✅ DENGAN RATIONALE:
"Kami mempertimbangkan hamburger menu, tapi riset Nielsen 
Norman menunjukkan bahwa fitur yang tersembunyi digunakan 
2-3x lebih jarang. Karena 4 fitur utama kami (Bayar, 
Transfer, Riwayat, Profil) semua sering diakses, tab bar 
lebih tepat meskipun membatasi jumlah item."
```

---

## 💻 Praktik: Format Presentasi

### Tips Presentasi Live Prototipe

```
SEBELUM PRESENTASI:
□ Test prototipe sekali lagi di device yang akan dipakai
□ Buka Figma prototype di Presentation Mode (bukan Edit Mode)
□ Matikan notifikasi
□ Siapkan backup: screenshot semua layar di folder terpisah

SAAT PRESENTASI:
1. MULAI DARI KONTEKS:
   "Kami mendesain untuk [persona] yang menghadapi [masalah].
   Sebelum demo, biarkan saya ceritakan apa yang kami pelajari..."
   
2. DEMO YANG TERFOKUS:
   - Jangan demo semua — pilih 1-2 flow yang paling kuat
   - "Biarkan saya tunjukkan bagaimana [persona] akan [task]"
   
3. JELASKAN KEPUTUSAN SAAT DEMO:
   "Di sini Anda lihat kami menempatkan konfirmasi dengan 
   detail lengkap — ini berdasarkan temuan bahwa 4 dari 5 
   pengguna kami takut salah transfer"

4. SIAP UNTUK PERTANYAAN:
   - "Mengapa memilih warna ini?" → tunjukkan contrast ratio
   - "Mengapa flow seperti ini?" → referensikan data riset
   - "Bagaimana jika [edge case]?" → "Itulah salah satu hal 
     yang akan kami test di iterasi berikutnya"
```

### Format untuk Portofolio

```
ENTRY PORTOFOLIO DI BEHANCE / PORTFOLIO WEBSITE:

1. COVER IMAGE yang menarik perhatian:
   - Bukan screenshot Figma default
   - Mockup di device yang realistis
   - Tagline singkat (1 kalimat problem statement)

2. STRUKTUR YANG MUDAH DI-SCAN:
   - Gunakan section yang jelas dengan heading
   - Banyak visual, sedikit teks panjang
   - Quote dari pengguna yang impactful

3. LENGTH:
   - Baca: 5-8 menit adalah ideal
   - Rekruter rata-rata 30 detik untuk decide apakah lanjut baca
   - Opening visual + 3 kalimat pertama SANGAT penting

4. METRICS:
   - SUS Score before/after
   - Task completion rate
   - Jumlah pengguna yang diteliti
   - Timeline proyek

5. PROCESS PHOTOS:
   - Foto saat melakukan user interview (dengan izin)
   - Foto sticky notes di whiteboard
   - Screenshot FigJam affinity diagram
   → Membuktikan prosesnya nyata, bukan dibuat-buat
```

---

## 🏢 Panduan Presentasi Proyek Final

### Alokasi Waktu (15 menit + Q&A)

```
00:00–01:00  Problem Statement & Target Pengguna
01:00–03:00  Research Process & Key Insights
03:00–05:00  Persona & Journey Map (yang paling relevan)
05:00–06:00  IA & User Flow
06:00–08:00  Design Evolution (Lo-Fi → Hi-Fi)
08:00–10:00  DEMO PROTOTIPE LIVE (focus 2 flow)
10:00–12:00  Usability Testing: Proses & Hasil
12:00–14:00  Iterasi & Outcomes
14:00–15:00  Lessons Learned & Next Steps
15:00+       Q&A
```

### Pertanyaan yang Mungkin Keluar di Q&A

| Pertanyaan | Cara Menjawab |
|---|---|
| "Mengapa desain seperti ini?" | Referensikan data riset atau heuristik |
| "Apakah kamu sudah test dengan pengguna nyata?" | Ceritakan metodologi UT |
| "Bagaimana jika user melakukan X?" | Tunjukkan error state atau flow alternatif |
| "Apa yang akan kamu ubah jika ada waktu lebih?" | Sebut 1-2 Big Bet yang belum sempat dieksekusi |
| "Bagaimana ini dibandingkan kompetitor?" | Kalau ada, tunjukkan competitive analysis |

---

## ⚠️ Kesalahan Umum dalam Presentasi Case Study

1. **Terlalu fokus pada visual, lupa proses** → "Ini desainnya, cantik kan?" bukan case study. Proses lebih penting dari hasil.

2. **Tidak ada data** → "Pengguna merasa lebih mudah" tanpa angka tidak meyakinkan. SUS score, task completion rate, quotes — semua harus ada.

3. **Demo yang stuck atau error** → Uji prototipe berkali-kali sebelum presentasi. Siapkan backup.

4. **Tidak tahu menjawab "mengapa"** → Setiap keputusan desain harus bisa dipertanggungjawabkan. Latih menjawab "mengapa" untuk setiap elemen.

5. **Terlalu panjang** → 15 menit adalah batas. Presenter yang baik memilih apa yang paling penting, bukan menceritakan segalanya.

---

## 🧪 Soal Latihan

### Soal 1 — Konsep

a) Mengapa Design Case Study yang menunjukkan proses lebih berharga dari yang hanya menunjukkan hasil akhir?

b) Bagaimana cara menjawab pertanyaan "mengapa kamu memilih [keputusan desain X]?" secara efektif?

### Soal 2 — Persiapan Presentasi Final

1. Siapkan **slide deck** atau **Figma presentation** dengan 8 section Design Case Study
2. Latih presentasi mandiri dengan timer — pastikan selesai dalam 14 menit (1 menit buffer)
3. Simulasikan 3 pertanyaan sulit dan siapkan jawabannya
4. Minta feedback dari teman atau dosen untuk propotipe demo sebelum hari presentasi

---

## 📌 Ringkasan

* **Design Case Study** = ceritakan MENGAPA dan BAGAIMANA, bukan hanya APA
* 8 section: Problem → Research → Define → IA → Iterations → Design System → UT → Outcomes
* **Storytelling kuat:** data kuantitatif + quotes pengguna + before/after
* **Demo live prototipe:** fokus pada 1-2 flow yang paling illustrasi keputusan desain
* Jelaskan **trade-offs** yang diambil — tidak ada jawaban yang selalu benar, yang penting justifikasinya
* Portofolio: visual kuat + metrics + process photos
* Q&A: setiap keputusan desain harus bisa direferensikan ke data atau framework

---

*📚 Referensi: Krug, S. (2014). Don't Make Me Think | UX Collective — How to Write a UX Case Study | Behance — UX Portfolio Best Practices*
